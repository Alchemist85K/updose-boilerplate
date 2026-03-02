# updose-boilerplate/git

Git rules and skills for AI coding agents.

## Install

```bash
npx updose add Alchemist85k/updose-boilerplate/git
```

## What's Included

### Rules (always active)

| File | Description |
|---|---|
| `git.md` | Safety rules: never auto-commit, never auto-push, never stage secrets, block dangerous commands |
| `gitflow.md` | Branch structure: main, develop, feature/*, release/*, hotfix/* |

### Skills (triggered on demand)

| Skill | Trigger | Description |
|---|---|---|
| `git-commit` | "commit", "커밋" | Lint/format check → stage → conventional commit message → commit |
| `git-pr` | "pr", "pull request" | Analyze diff → generate title/body → create PR via `gh` |
| `git-branch` | "branch", "feature", "release", "hotfix", "gitflow" | Create and finish branches following gitflow workflow |

## Requirements

- Git
- [GitHub CLI (gh)](https://cli.github.com/) — for PR creation
