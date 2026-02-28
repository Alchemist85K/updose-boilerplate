# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Mandatory Rules (Never Forget)

**1. Only perform requested tasks:**

- **Never** proceed with unrequested work
- If you want to suggest additional work, propose it **after completing the requested task** (never act on your own)
- Only modify the files you were asked to modify, even if similar code exists elsewhere

**2. Always run lint and format after every code change.** Fix all errors before proceeding. Skip for documentation-only changes. Skip if not configured.

---

## Documentation Management Rules

**docs/plans/**:

- Filename format: `[feature]-YYYY-MM-DD.md`
- Every plan file must include a checklist
- Do not modify documents where all checklist items are completed
- Create a new file if a new plan is needed for the same feature
- Multiple plans on the same day: `[feature]-YYYY-MM-DD-1.md`, `[feature]-YYYY-MM-DD-2.md`

**docs/review/**:

- Document reviews, retrospectives, and lessons learned
- Filename format: `[topic]-YYYY-MM-DD.md`
- Include: summary, what went well, what needs improvement, action items

**docs/ARCHITECTURE.md, docs/DEVELOPMENT.md**:

- Keep up to date when code changes, after confirming with the user
