# updose-boilerplate

A collection of [updose](https://updose.dev) boilerplates for AI coding agents. Each boilerplate provides pre-configured rules, skills, and conventions for Claude Code.

## Boilerplates

| Boilerplate | Description | Install |
|---|---|---|
| [`git`](./git) | Git rules and skills — conventional commits, PR creation, gitflow branch management | `npx updose add Alchemist85K/updose-boilerplate/git` |
| [`nextjs`](./nextjs) | Next.js 16+ App Router rules and skills — security, React best practices, Tailwind v4, Vitest, frontend design | `npx updose add Alchemist85K/updose-boilerplate/nextjs` |
| [`supabase-js`](./supabase-js) | Supabase security rules and Postgres best practices | `npx updose add Alchemist85K/updose-boilerplate/supabase-js` |
| [`fastapi`](./fastapi) | FastAPI + AI rules — async-first, Pydantic v2, SSE streaming, Anthropic SDK patterns | `npx updose add Alchemist85K/updose-boilerplate/fastapi` |

## Usage

```bash
npx updose add Alchemist85K/updose-boilerplate/<boilerplate-name>
```

Boilerplates can be combined. For example, to set up a Next.js project with git conventions and Supabase:

```bash
npx updose add Alchemist85K/updose-boilerplate/git
npx updose add Alchemist85K/updose-boilerplate/nextjs
npx updose add Alchemist85K/updose-boilerplate/supabase-js
```
