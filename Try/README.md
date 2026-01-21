# try - Complete Feature Specification

## Overview
`try` is an ephemeral workspace manager that organizes experimental project directories with intelligent fuzzy search, time-aware sorting, and seamless shell integration. It provides a terminal user interface (TUI) for navigating, creating, and managing "try" directories.

## Core Functionality

### Directory Management
- **Centralized Storage**: All experiments live in a single root directory (`~/src/tries` by default, configurable via `TRY_PATH`)
- **Auto-dating**: New directories automatically prefixed with `YYYY-MM-DD-` format
- **Unique Naming**: Versioned naming when conflicts occur (`project-2`, `project-3`)
- **Batch Operations**: Mark multiple directories for deletion with confirmation

### Time-Aware Organization
- **Recency Bonus**: Recently accessed directories score higher in search results
- **Relative Timestamps**: Display "just now", "2h ago", "3d ago", "2w ago"
- **Automatic Sorting**: Directories bubble up based on last access time

### Git Integration
- **Clone Support**: Clone repositories into date-prefixed directories with automatic naming
- **Worktree Support**: Create detached git worktrees from existing repositories
- **URL Parsing**: Supports GitHub, GitLab, SSH, and HTTPS URLs
- **Custom Names**: Optionally override auto-generated directory names

## TUI (Terminal User Interface)

### Layout
- **Responsive Design**: Adapts to terminal window size changes
- **Three Sections**: Header (title + search), Body (directory list), Footer (help/status)
- **Dynamic Sizing**: List area adjusts based on available terminal rows
- **Two-Layer Display**: Left-aligned directory names, right-aligned metadata

### Search & Filtering
- **Real-time Fuzzy Search**: Matches characters in sequence (e.g., "rds" matches "redis-server")
- **Query Highlighting**: Matched characters highlighted in bold yellow
- **Instant Feedback**: Results update with each keystroke
- **Create New Option**: Non-matching queries show "[new]" entry for immediate creation

### Visual Features
- **Date Prefix Dimming**: `YYYY-MM-DD-` prefix displayed in muted gray
- **Selection Indicator**: `→` marks currently selected item
- **Delete Marking**: Items marked for deletion show with dark red background
- **Metadata Display**: Shows relative timestamp and fuzzy score when space permits
- **Smart Truncation**: Long names truncated with ellipsis, preserving highlighting tokens

### Keyboard Navigation
#### General Navigation
- `↑` / `↓` or `Ctrl-P` / `Ctrl-N`: Move selection up/down
- `Enter`: Select current item
- `Esc` / `Ctrl-C`: Cancel and exit
- `Ctrl-D`: Enter delete mode / toggle mark

#### Search Input Editing
- `Ctrl-A`: Move cursor to beginning of line
- `Ctrl-E`: Move cursor to end of line
- `Ctrl-B` / `Ctrl-F`: Move cursor backward/forward
- `Backspace` / `Ctrl-H`: Delete character before cursor
- `Ctrl-K`: Delete from cursor to end of line
- `Ctrl-W`: Delete previous word (alphanumeric boundaries)

#### Delete Mode
- `Ctrl-D`: Toggle mark on current directory
- `Enter`: Show confirmation dialog for marked items
- `Esc`: Exit delete mode, clear all marks

## CLI Commands

### `try` / `try cd [query]` (Default)
- **Interactive Selector**: Launch TUI for directory selection
- **Initial Filter**: Optional query pre-filters results
- **Output**: Shell script to `cd` into selected directory
- **Actions**: Select existing directory, create new, or cancel

### `try clone <url> [name]`
- **Git Cloning**: Clone repository into date-prefixed directory
- **Auto-naming**: Extracts user/repo from URL (e.g., `2025-11-30-tobi-try`)
- **Custom Name**: Optional name overrides auto-generated suffix
- **URL Shorthand**: `try <url>` works without "clone" command

### `try worktree [repo] [name]`
- **Git Worktrees**: Create detached worktree from existing repository
- **Repository Source**: Current directory by default, or specified path
- **Directory Creation**: Creates `YYYY-MM-DD-<name>` directory
- **Shorthand**: `try . [name]` for current repository

### `try init [path]`
- **Shell Integration**: Outputs shell function definition
- **Shell Detection**: Auto-detects bash/zsh vs fish
- **Path Embedding**: Embeds absolute path to try binary and tries directory
- **Installation**: Users `eval` output in shell config

### `try exec <command>`
- **Internal Command**: Used by shell wrapper to emit shell scripts
- **Script Output**: Returns shell commands for evaluation
- **Exit Codes**: 0=success (eval), 1=error/cancel (print)

## Fuzzy Matching Algorithm

### Scoring Components
1. **Character Match**: +1.0 per matched character
2. **Word Boundary Bonus**: +1.0 if match starts at word beginning
3. **Proximity Bonus**: +2.0 / √(gap + 1) for consecutive matches
4. **Density Multiplier**: `query_length / (last_match_position + 1)`
5. **Length Penalty**: `10 / (name_length + 10)`
6. **Date Prefix Bonus**: +2.0 for `YYYY-MM-DD-` prefixed names
7. **Recency Bonus**: +3.0 / √(hours_since_access + 1)

### Filtering Rules
- All query characters must be found in sequence
- Zero score entries are hidden
- Case-insensitive matching

### Example Scoring
```
Directory: "2025-11-29-project"
Query: "pro"
Last accessed: 1 hour ago

Fuzzy score: 8.0
Density: ×0.214
Length: ×0.526
Fuzzy after multipliers: 0.90
Date bonus: +2.0
Recency bonus: +2.1
Final score: ~5.0
```

## Shell Integration

### Wrapper Function
- **Purpose**: Enables `cd` commands to affect parent shell
- **Implementation**: Captures `try exec` output, `eval`s on success
- **Error Handling**: Prints output on cancellation/error

### Supported Shells
- **Bash/Zsh**: POSIX function syntax
- **Fish**: Fish-specific function syntax

### Installation
```bash
# Bash/Zsh
eval "$(try init ~/src/tries)"

# Fish
eval (try init ~/src/tries | string collect)
```

## Safety Features

### Delete Safety
- **Path Containment**: Verifies deletions stay within tries directory
- **Realpath Validation**: Resolves symlinks before deletion
- **Confirmation**: Requires typing "YES" exactly
- **PWD Protection**: Changes directory if current path would be deleted

### Script Safety
- **Quoting**: Proper shell quoting for paths with spaces/special characters
- **Existence Checks**: `test -d` before `rm -rf`
- **Error Handling**: Continues on failure, falls back to `$HOME`

### Configuration Safety
- **Environment Variables**: `TRY_PATH` controls root, `NO_COLOR` disables colors
- **Default Boundaries**: Never writes outside user-specified paths

## Configuration

### Environment Variables
- `TRY_PATH`: Root directory for tries (default: `~/src/tries`)
- `NO_COLOR`: Disable ANSI colors (any non-empty value)
- `SHELL`: Used by `init` for shell detection

### Command Line Flags
- `--path <dir>`: Override tries directory for single command
- `--no-colors`: Disable color output
- `--version` / `-v`: Show version
- `--help` / `-h`: Show help

## Testing Framework

### Spec System
- **Markdown Specs**: Human-readable specifications in `spec/*.md`
- **Shell Tests**: Automated test suite in `spec/tests/`
- **Validation**: Any implementation must pass all spec tests

### Test Runner
- **Cross-language**: Tests work with Ruby, Rust, or any implementation
- **Comprehensive**: Covers CLI, TUI, fuzzy matching, edge cases
- **CI Integration**: GitHub Actions runs tests on push

### Test Utilities
- **Key Injection**: Simulate keyboard input for TUI tests
- **Environment Control**: Set terminal size, colors, paths
- **Output Validation**: Verify shell script format and behavior

## Token System

### Formatting Tokens
- `{b}` / `{/b}`: Bold yellow (highlight)
- `{dim}` / `{/fg}`: Gray (dimmed)
- `{strike}` / `{/strike}`: Dark red background (delete)
- `{h1}` / `{h2}`: Heading styles
- `{reset}`: Full reset

### Usage
- **Declarative**: Styling defined in data, not code
- **Centralized**: Consistent appearance across UI
- **Expandable**: Unknown tokens pass through unchanged

## Directory Scoring Algorithm (Detailed)

### Base Score Calculation
```ruby
def compute_base_score(basename, mtime)
  hours_since_access = (Time.now - mtime) / 3600.0
  score = 3.0 / Math.sqrt(hours_since_access + 1)
  
  # Date prefix bonus
  if basename.match?(/^\d{4}-\d{2}-\d{2}-/)
    score += 2.0
  end
  
  score
end
```

### Fuzzy Match Implementation
1. **Sequential Matching**: Find each query character in order
2. **Position Tracking**: Record match positions for highlighting
3. **Gap Calculation**: Distance between consecutive matches
4. **Score Accumulation**: Apply bonuses per match
5. **Multiplier Application**: Density and length penalties
6. **Contextual Addition**: Date and recency bonuses

### Sorting Behavior
- **No Query**: Sort by (date_bonus + recency_bonus)
- **With Query**: Sort by final score descending
- **Equal Scores**: No guaranteed order (implementation-defined)

## File System Operations

### Directory Creation
- **Path Construction**: `File.join(base_path, "#{date_prefix}-#{name}")`
- **Unique Resolution**: Appends `-2`, `-3` if name exists
- **Git Integration**: Adds worktree if within git repository

### Directory Deletion
- **Batch Processing**: Multiple directories in single operation
- **Safety Check**: Realpath verification
- **Cleanup**: Removes directory and all contents

### Directory Renaming
- **Validation**: Name cannot contain `/`, must be unique
- **Preservation**: Maintains date prefix if present
- **Atomic Operation**: `mv` command in shell script

## Performance Considerations

### Caching
- **Directory Scan**: Single pass through tries directory
- **Stat Cache**: File metadata cached per session
- **Fuzzy Index**: Pre-processed text for matching

### Optimization
- **Fast Paths**: ASCII-only strings use optimized width calculation
- **Memoization**: Repeated queries reuse previous results
- **Lazy Loading**: Directory scanning deferred until needed

### Scalability
- **Thousands of Directories**: Efficient scoring algorithm
- **Memory Usage**: Minimal per-entry storage
- **Response Time**: Instant feedback for typing

## Cross-Platform Compatibility

### Operating Systems
- **macOS**: Built-in Ruby support
- **Linux**: Standard Ruby installation
- **Windows**: WSL/Cygwin compatibility

### Terminal Support
- **ANSI Escape Codes**: Standard terminal sequences
- **Unicode**: Emoji and special characters
- **Window Resize**: SIGWINCH signal handling

### Shell Compatibility
- **Bash 3.2+**: POSIX-compatible syntax
- **Zsh**: Extended shell features
- **Fish 3.0+**: Modern shell syntax

## Extensibility

### New Commands
- **Plugin Architecture**: Add custom commands via subcommands
- **Configuration Files**: Future support for `.tryrc`

### Custom Scoring
- **Weight Tuning**: Adjust bonus multipliers
- **Custom Patterns**: Additional scoring rules

### UI Customization
- **Color Themes**: Alternative palette support
- **Layout Options**: Custom header/footer content

## Philosophy & Design Principles

### User Experience
- **Zero Configuration**: Works immediately after installation
- **Progressive Disclosure**: Advanced features available but not required
- **Forgiving Interface**: Easy recovery from mistakes

### Code Quality
- **Single File**: Ruby implementation is one executable file
- **No Dependencies**: Uses only Ruby standard library
- **Readable Code**: Clear naming and structure

### Community Values
- **ADHD-Friendly**: Designed for scattered focus and context switching
- **Open Source**: MIT licensed, welcoming contributions
- **Practical Tool**: Solves real problems for developers

---
*Last updated: Based on specification v1.7.1*
