# LLM Wiki Schema

This document defines how this wiki is structured, maintained, and operated. Follow these conventions when ingesting sources, answering queries, or linting the wiki.

## Directory Structure

```
.
├── raw/              # Immutable source documents (you own this)
│   ├── assets/       # Downloaded images and attachments
│   └── ...           # Articles, papers, transcripts, notes
├── wiki/             # LLM-generated knowledge pages (LLM owns this)
│   ├── index.md      # Catalog of all wiki pages
│   ├── log.md        # Chronological record of operations
│   ├── _templates/   # Reusable page templates
│   └── ...           # Entity pages, concept pages, summaries
└── AGENTS.md         # This file — the schema
```

## Page Types

Use these page types consistently. Each page should declare its type in YAML frontmatter.

| Type        | Purpose                              | Example                                  |
| ----------- | ------------------------------------ | ---------------------------------------- |
| `source`    | Summary of a raw source document     | `[[Source - Attention Is All You Need]]` |
| `entity`    | Person, organization, product, place | `[[Entity - OpenAI]]`                    |
| `concept`   | Idea, theory, technique, framework   | `[[Concept - Transformer Architecture]]` |
| `synthesis` | Cross-source analysis or comparison  | `[[Synthesis - LLM Scaling Laws]]`       |
| `index`     | Directory / catalog page             | `[[index]]`                              |
| `log`       | Chronological record                 | `[[log]]`                                |

## Frontmatter Convention

Every wiki page must include YAML frontmatter:

```yaml
---
title: "Page Title"
type: entity | concept | source | synthesis | index | log
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: # for entity/concept/synthesis pages
  - "[[Source - Article Title]]" # list of source pages that contributed
tags:
  - tag1
  - tag2
---
```

## Ingest Workflow

When a new source arrives in `raw/`:

1. **Read** the source file(s) thoroughly.
2. **Discuss** key takeaways with the user. Ask what to emphasize.
3. **Create** a `source` page in `wiki/` summarizing the document.
   - Filename: match the source filename but in `wiki/`
   - Title prefix: `Source - ` for clarity
   - Include key claims, quotes, and a "Key Takeaways" section
4. **Update** relevant `entity` and `concept` pages:
   - Add new information
   - Note contradictions with existing claims
   - Update the `updated` date and `sources` list in frontmatter
5. **Create** new `entity` or `concept` pages if important new ones appear.
6. **Update** `wiki/index.md` with the new page(s).
7. **Append** an entry to `wiki/log.md`.

## Query Workflow

When the user asks a question:

1. **Read** `wiki/index.md` to locate relevant pages.
2. **Read** the relevant wiki pages (not raw sources, unless needed for verification).
3. **Synthesize** an answer with citations to wiki pages, e.g. `[[Concept - Foo]]`.
4. **If valuable**, offer to file the answer as a new `synthesis` page in `wiki/`.
   - Good candidates: comparisons, analyses, connections discovered during exploration
   - Filed answers compound the knowledge base instead of disappearing into chat history

## Lint Workflow

Run periodically (suggest weekly or after every ~10 ingests):

1. **Contradictions** — scan for claims on different pages that conflict. Flag them.
2. **Stale claims** — identify claims superseded by newer sources.
3. **Orphans** — find pages with zero inbound wiki-links.
4. **Gaps** — note important concepts mentioned but lacking their own page.
5. **Missing cross-references** — suggest links that should exist.
6. **Index drift** — ensure `index.md` matches actual wiki pages.
7. **Append** a lint report entry to `wiki/log.md`.

## Cross-Reference Convention

- Use Obsidian wiki-links: `[[Page Title]]`
- Link generously. Every mention of an entity or concept should link to its page.
- When creating a new page, add at least one inbound link from an existing page.
- Prefer `[[Concept - Foo|Foo]]` for readable display text when the prefix is noisy.

## File Naming

- Use Title Case with spaces: `Concept - Transformer Architecture.md`
- Prefix page type for clarity: `Source -`, `Entity -`, `Concept -`, `Synthesis -`
- Match the `title` frontmatter field exactly to the filename (without `.md`)

## Log Format

Each log entry must start with a consistent prefix:

```markdown
## [YYYY-MM-DD] <operation> | <subject>

- Details...
- Pages touched: [[Page1]], [[Page2]]
```

Operations: `ingest`, `query`, `lint`, `create`, `update`

This makes the log parseable with simple tools:

```bash
grep "^## \[" wiki/log.md | tail -5
```

## Tips

- The wiki is just a git repo of markdown files. Version history is free.
- Obsidian graph view shows wiki shape — use it to spot orphans and hubs.
- If a page grows too long, split it and create a summary `Synthesis -` page.
- Prefer updating existing pages over creating new ones when information overlaps.
