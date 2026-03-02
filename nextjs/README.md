# updose-boilerplate/nextjs

A pre-configured Claude Code boilerplate for **Next.js 16+ App Router** projects. Provides rules and curated AI skills for AI coding agents.

## Install

```bash
npx updose add Alchemist85K/updose-boilerplate/nextjs
```

## What's Included

### Claude Code Configuration

- **CLAUDE.md** — Core development rules (task scoping, lint/format enforcement, documentation management)

### Rules (always active)

| File | Description |
|---|---|
| `nextjs.md` | Next.js 16+ App Router, security (NEXT_PUBLIC_, Server Actions auth, proxy.ts), async params, built-in components |

### External Skills (via skills.json)

| Skill | Description |
|---|---|
| [`context7`](https://skills.sh/intellectronica/agent-skills/context7) | Up-to-date library documentation lookup |
| [`next-best-practices`](https://skills.sh/vercel-labs/next-skills/next-best-practices) | Next.js best practices and patterns |
| [`next-cache-components`](https://skills.sh/vercel-labs/next-skills/next-cache-components) | Next.js caching and component strategies |
| [`vercel-composition-patterns`](https://skills.sh/vercel-labs/agent-skills/vercel-composition-patterns) | React composition patterns |
| [`vercel-react-best-practices`](https://skills.sh/vercel-labs/agent-skills/vercel-react-best-practices) | React best practices |
| [`tailwind-design-system`](https://skills.sh/wshobson/agents/tailwind-design-system) | Tailwind CSS design system patterns |
| [`web-design-guidelines`](https://skills.sh/vercel-labs/agent-skills/web-design-guidelines) | Web design guidelines and principles |
| [`frontend-design`](https://skills.sh/anthropics/skills/frontend-design) | Frontend design best practices |
| [`vitest`](https://skills.sh/antfu/skills/vitest) | Vitest testing patterns |

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

`nextjs` `react` `typescript` `tailwindcss` `web`
