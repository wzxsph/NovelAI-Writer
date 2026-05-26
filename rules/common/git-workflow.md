---
name: git-workflow
description: Git 工作流规范
---

# Git Workflow

## Commit Message Format

```
<type>: <description>

<optional body>
```

Types: feat, fix, refactor, docs, test, chore, perf, ci

## Pull Request Workflow

1. Analyze full commit history
2. Use `git diff [base-branch]...HEAD` to see all changes
3. Draft comprehensive PR summary
4. Include test plan with TODOs

## Branch Naming
- feature/xxx-description
- fix/xxx-issue
- refactor/xxx-component
