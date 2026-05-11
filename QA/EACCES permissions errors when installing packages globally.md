## Background

When executing the pre-commit hook using `git commit`, the globally installed `prettier` command could not be found.

_pre-commit_ hook

```sh
#!/bin/sh
FILES=$(git diff --cached --name-only --diff-filter=ACMR | sed 's| |\\ |g')
[ -z "$FILES" ] && exit 0

# Prettify all selected files
echo "$FILES" | xargs prettier --ignore-unknown --write

# Add back the modified/prettified files to staging
echo "$FILES" | xargs git add

exit 0
```


```bash
$ git commit -m "vault backup: $(date +'%F %T')"
xargs: prettier: No such file or directory
[main 98e68f5] vault backup: 2026-05-11 23:45:50
 2 files changed, 18 insertions(+)
 create mode 100644 QA/EACCES permissions errors when installing packages globally.md
```

## Question

```bash
npm install --global prettier
```

```
npm error code EACCES
npm error syscall mkdir
npm error path /usr/local/lib/node_modules
npm error errno -13
npm error Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules'
npm error     at async mkdir (node:internal/fs/promises:858:10)
npm error     at async /usr/lib/node_modules_22/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:638:20
npm error     at async Promise.allSettled (index 0)
npm error     at async [reifyPackages] (/usr/lib/node_modules_22/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:334:11)
npm error     at async Arborist.reify (/usr/lib/node_modules_22/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:149:5)
npm error     at async Install.exec (/usr/lib/node_modules_22/npm/lib/commands/install.js:150:5)
npm error     at async Npm.exec (/usr/lib/node_modules_22/npm/lib/npm.js:207:9)
npm error     at async module.exports (/usr/lib/node_modules_22/npm/lib/cli/entry.js:74:5) {
npm error   errno: -13,
npm error   code: 'EACCES',
npm error   syscall: 'mkdir',
npm error   path: '/usr/local/lib/node_modules'
npm error }
npm error
npm error The operation was rejected by your operating system.
npm error It is likely you do not have the permissions to access this file as the current user
npm error
npm error If you believe this might be a permissions issue, please double-check the
npm error permissions of the file and its containing directories, or try running
npm error the command again as root/Administrator.
npm error A complete log of this run can be found in: /home/yshngg/.npm/_logs/2026-05-11T15_50_00_884Z-debug-0.log
```
## Answer

### Option A

reinstall npm with a node version manager
### Option B

```bash
npm config set prefix ~/.local

PATH=~/.local/bin:$PATH
```

Ref:
https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally