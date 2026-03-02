---
name: git-pr
description: 'Create a GitHub Pull Request from the current branch. Use when the user asks to create a PR, open a pull request, or mentions "pr". Analyzes branch diff, auto-generates title and description following conventional commit format, and creates the PR via GitHub CLI (gh).'
allowed-tools: Bash
---

# Git PR Procedure

Follow these steps when the user requests a PR.

## 1. Prerequisites Check

### GitHub CLI

```bash
command -v gh >/dev/null 2>&1
```

If not installed, abort and show:

```
❌ GitHub CLI (gh) is not installed.

Install:
- macOS: brew install gh
- Ubuntu: https://github.com/cli/cli/blob/trunk/docs/install_linux.md
- Windows: winget install --id GitHub.cli

Then run: gh auth login
```

### Uncommitted Changes

```bash
git status --porcelain
```

If uncommitted changes exist, abort and show:

```
❌ Uncommitted changes detected. Commit or stash before creating a PR.
```

### Existing PR

```bash
gh pr view --json url 2>/dev/null
```

If a PR already exists for this branch, show the URL and abort.

## 2. Current State

```bash
git branch --show-current
git log origin/main..HEAD --oneline
```

## 3. Push

```bash
git status -sb
```

- No remote branch → `git push -u origin <branch>`
- Unpushed commits → `git push`

## 4. Determine Base Branch

Detect base branch from branch name prefix:

| Current Branch | Base |
|---|---|
| `feature/*` | `develop` |
| `develop` | `main` |
| `release/*`, `hotfix/*` | `main` |
| Other | Ask user |

## 5. Analyze Changes

```bash
git diff <base>...HEAD --stat
git log <base>..HEAD --oneline
```

## 6. Generate Title and Body

**Title format**: `<type>: <description>`

Types: `feat`, `fix`, `refactor`, `docs`, `chore`, `test`, `perf`, `ci`, `build`

- Infer type from branch prefix or commit history.
- Under 50 characters, imperative mood.

**Body format**:

```markdown
## Summary

- High-level summary (1-3 bullets)

## Changes

- Key changes with file/module context
```

## 7. Create PR

```bash
gh pr create --base <base> --title "<title>" --body "$(cat <<'EOF'
## Summary

- ...

## Changes

- ...
EOF
)"
```

If user specified `--draft`, add `--draft` flag.

## 8. Report Result

```
## PR Created

**URL**: [PR URL]
**Title**: [PR title]
**Base**: [base] ← [current branch]

### Commits
- [commit list]
```
