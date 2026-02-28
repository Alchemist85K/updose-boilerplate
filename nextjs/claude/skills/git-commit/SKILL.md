---
name: git-commit
description: 'Execute git commit with conventional commit messages, lint/format checks, intelligent staging, and message generation. Use when the user asks to commit changes, create a git commit, or mentions "commit". Supports: (1) Pre-commit lint and format checks with auto-fix, (2) Auto-detecting type and scope from diff, (3) Generating conventional commit messages, (4) Staging with sensitive file protection. Always use this skill whenever the user mentions committing, even if they just say "commit this" or "commit".'
allowed-tools: Bash
---

# Git Commit Procedure

Follow these steps when the user requests a commit.

## 1. Review Changes

Identify the full scope of changes before proceeding.

```bash
git status --porcelain
git diff --staged
git diff
```

## 2. Quality Checks

Run the project's lint and format commands before committing.

- If the project has lint or format commands, run them. Auto-fix when possible (lint --fix, format). If unfixable errors remain, abort the commit and report to the user. If no lint/format config exists, skip this step.
- Always fix all lint/format errors before committing, even if they are unrelated to the current changes. Include those fixes in the same commit.

## 3. Stage Changes

- Stage unstaged changes with `git add`
- Always include formatting fixes (lint --fix, format results) in the same commit. Do not split them into a separate commit.
- NEVER stage files containing `.env`, `.env.local`, credentials, secrets, or API keys. If found, abort the commit, warn the user, and recommend adding them to `.gitignore`.
- Recommend `.gitignore` additions for build artifacts, log files, and other files that should not be committed.
- Do not commit if there are no staged changes.

## 4. Commit Message

Follow the **Conventional Commits** specification.

**Types**:

- `feat`: new feature
- `fix`: bug fix
- `refactor`: code restructuring without behavior change
- `perf`: performance improvement
- `style`: formatting, semicolons, etc. (no logic change)
- `docs`: documentation only
- `test`: add or update tests
- `build`: build system or external dependency changes
- `ci`: CI configuration changes
- `chore`: maintenance, miscellaneous tasks
- `revert`: revert a previous commit

**Format**:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Simple change (1-2 files, single purpose):

```
<type>: <description>
```

Complex change (multiple files, multiple concerns):

```
<type>(<scope>): <description>

- detail 1
- detail 2
```

**Scope**:

Indicate the affected module/area in parentheses. Optional, but useful in monorepos or multi-module projects.

```
feat(auth): add OAuth2 login
fix(api): handle null response from payment endpoint
docs(readme): update installation guide
```

**Breaking Changes**:

Always mark changes that break backward compatibility.

```
# ! after type/scope
feat!: remove deprecated endpoint

# Or BREAKING CHANGE footer
feat: allow config to extend other configs

BREAKING CHANGE: `extends` key behavior changed
```

**Writing Rules**:

- Subject line: under 50 chars, English, imperative mood ("add", "fix", "update")
- Body lines: wrap at 72 chars
- Clearly convey what changed and why
- Use HEREDOC for multi-line messages to prevent format issues

**Executing the Commit**:

```bash
# Single line
git commit -m "<type>: <description>"

# Multi-line with body/footer
git commit -m "$(cat <<'EOF'
<type>(<scope>): <description>

<optional body>

<optional footer>
EOF
)"
```

## 5. Report Result

After committing, report in this format:

```
## Commit Complete

**Hash**: [first 7 chars of commit hash]
**Message**: [commit message subject]

### Changed Files
- [file list]
```
