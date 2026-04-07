---
name: git-branch
description: 'Manage git branches following gitflow workflow: create, finish, and clean up feature, release, and hotfix branches. Use when the user asks to create a branch, start a feature, make a release, create a hotfix, finish/close a branch, or mentions "gitflow".'
allowed-tools: Bash
---

# Git Branch Procedure

## Branch Naming

```
feature/<short-description>    e.g. feature/add-login
release/<version>              e.g. release/1.2.0
hotfix/<version>               e.g. hotfix/1.2.1
```

- Lowercase, kebab-case

## Create Branch

### feature

```bash
git checkout develop
git pull origin develop
git checkout -b feature/<name>
```

### release

```bash
git checkout develop
git pull origin develop
git checkout -b release/<version>
```

### hotfix

```bash
git checkout main
git pull origin main
git checkout -b hotfix/<version>
```

## Finish Branch

### feature

1. Merge to develop (via PR or merge)
2. Delete branch

```bash
git checkout develop
git merge feature/<name>
git branch -d feature/<name>
git push origin --delete feature/<name>
```

### release

1. Check if an open PR exists for the branch
2. If PR exists, merge the PR first (`gh pr merge`)
3. Pull main to reflect the merged PR
4. Tag with version
5. Merge to develop
6. Delete branch

**With open PR:**

```bash
# 1. Merge PR (into main)
gh pr merge release/<version> --merge

# 2. Update local main
git checkout main
git pull origin main

# 3. Tag & merge to develop
git tag -a v<version> -m "Release <version>"
git checkout develop
git merge release/<version>
git branch -d release/<version>
git push origin --delete release/<version>
git push origin develop --tags
```

**Without PR (local merge):**

```bash
git checkout main
git merge release/<version>
git tag -a v<version> -m "Release <version>"
git checkout develop
git merge release/<version>
git branch -d release/<version>
git push origin --delete release/<version>
git push origin main develop --tags
```

### hotfix

1. Merge to main
2. Tag with version
3. Merge to develop
4. Delete branch

```bash
git checkout main
git merge hotfix/<version>
git tag -a v<version> -m "Hotfix <version>"
git checkout develop
git merge hotfix/<version>
git branch -d hotfix/<version>
git push origin --delete hotfix/<version>
git push origin main develop --tags
```

## Versioning

Follow **Semantic Versioning**:

```
MAJOR.MINOR.PATCH

MAJOR  → breaking changes
MINOR  → new features (backward compatible)
PATCH  → bug fixes (backward compatible)
```

## Report Result

After creating a branch:

```
## Branch Created

**Branch**: [branch name]
**From**: [source branch]
```

After finishing a branch:

```
## Branch Finished

**Branch**: [branch name]
**Merged to**: [target branches]
**Tag**: [version tag, if applicable]
**Deleted**: [branch name]
```
