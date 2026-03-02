# updose-boilerplate/fastapi

A pre-configured Claude Code boilerplate for **FastAPI + AI** project. Provides rules and best practices for AI coding agents.

## Install

```bash
npx updose add Alchemist85K/updose-boilerplate/fastapi
```

## What's Included

### Claude Code Configuration

- **CLAUDE.md** — Core development rules (task scoping, lint/format enforcement, documentation management)

### Rules (always active)

| File | Description |
|---|---|
| `fastapi.md` | Project structure, Pydantic v2, async-first, dependency injection, error handling, security, testing conventions |
| `ai.md` | AI client lifecycle (lifespan), SSE streaming, retry with backoff, token usage logging, prompt management |

### External Skills (via skills.json)

| Skill | Description |
|---|---|
| [`context7`](https://skills.sh/intellectronica/agent-skills/context7) | Up-to-date library documentation lookup |
| [`fastapi-templates`](https://skills.sh/wshobson/agents/fastapi-templates) | FastAPI project templates and patterns |
| [`api-design-principles`](https://skills.sh/wshobson/agents/api-design-principles) | RESTful API design best practices |
| [`async-python-patterns`](https://skills.sh/wshobson/agents/async-python-patterns) | Async Python patterns and concurrency |

### Documentation Structure

The boilerplate sets up conventions for project documentation:

- `docs/plans/` — Feature plans with checklists
- `docs/review/` — Reviews, retrospectives, and lessons learned
- `docs/specs/` — Feature specifications and behavior definitions
- `docs/ARCHITECTURE.md` — System architecture
- `docs/DEVELOPMENT.md` — Development guide

## Targets

- `Claude Code`

## Tags

`fastapi` `python` `pydantic` `ai`
