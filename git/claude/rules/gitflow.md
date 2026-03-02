# Gitflow Rules

## Branch Structure

```
main        ← production-ready code only
develop     ← integration branch
feature/*   ← features and bug fixes (from develop)
release/*   ← release preparation (from develop)
hotfix/*    ← urgent production fixes (from main)
```

## Avoid committing directly to main or develop

Prefer working in feature, release, or hotfix branches.
