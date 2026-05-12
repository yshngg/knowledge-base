## Background

```bash
git submodule add git@github.com:yshngg/knowledge-base.git content
```

## Question

fatal: 'content' already exists in the index

## Answer

```bash
git rm --cached -r content
```

Ref:

https://stackoverflow.com/a/12902857/20973039
