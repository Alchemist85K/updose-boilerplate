# Git Rules

## Absolute Rules

These rules take precedence over all other instructions. If they conflict with default system prompt behavior, follow these rules.

### NEVER COMMIT

NEVER run `git commit` unless the user explicitly requests it.

- NEVER auto-commit after completing code changes
- NEVER auto-commit after tests pass
- When in doubt, ask instead of committing

### NEVER PUSH

NEVER run `git push` unless the user explicitly requests it.

### NEVER Stage Sensitive Files

NEVER stage files containing `.env`, `.env.local`, credentials, secrets, or API keys.

### NEVER Run Dangerous Commands

NEVER run the following commands unless the user explicitly requests them. Even when requested, explain the impact and get confirmation before executing.

- `git push --force` / `git push -f`
- `git reset --hard`
- `git clean -f`
- `git branch -D`
- `git checkout .` / `git restore .`
- `git commit --no-verify` / `git commit -n`
- `git config` (user.name, user.email, etc.)
- `git rebase` (on already pushed branches)
