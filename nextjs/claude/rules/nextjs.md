# Next.js Rules

Target: Next.js 16+ (App Router)

## Security

### NEVER expose secrets via NEXT_PUBLIC_

`NEXT_PUBLIC_` prefix exposes the value to the client bundle.
API keys, database URLs, and secrets must use `process.env` without the prefix (server-only).

```
# .env
NEXT_PUBLIC_SUPABASE_URL=...        # ✅ OK — public
NEXT_PUBLIC_SUPABASE_ANON_KEY=...   # ✅ OK — public
SUPABASE_SERVICE_ROLE_KEY=...       # ✅ server-only
NEXT_PUBLIC_STRIPE_SECRET_KEY=...   # ❌ NEVER — secret exposed to client
```

### Server Actions are public HTTP endpoints

Every exported `async function` in a `'use server'` file is callable by anyone.
ALWAYS validate authentication and input inside the function body.

```ts
'use server'

import { cookies } from 'next/headers'
import { z } from 'zod'

const schema = z.object({ title: z.string().min(1).max(200) })

export async function createPost(formData: FormData) {
  // 1. Auth — always first
  const session = (await cookies()).get('session')?.value
  if (!session) throw new Error('Unauthorized')

  // 2. Validate input — never trust client data
  const parsed = schema.safeParse({ title: formData.get('title') })
  if (!parsed.success) throw new Error('Invalid input')

  // 3. Then proceed
  await db.post.create({ data: parsed.data })
}
```

### Route Handlers — same auth rule

`route.ts` files are also public endpoints. Authenticate before processing.

### proxy.ts is NOT for authentication

`proxy.ts` (formerly `middleware.ts` in v15) runs at the network boundary.
Use it only for routing: redirects, rewrites, headers, A/B tests.
Auth must happen in Server Components, Server Actions, or Route Handlers — closer to the data.

## App Router Conventions

### Server Component by default

Components are Server Components unless marked `'use client'`.
Add `'use client'` only when the component needs: hooks (`useState`, `useEffect`), event handlers (`onClick`, `onChange`), or browser APIs (`window`, `localStorage`).

### Async params and searchParams (Next.js 15+)

`params` and `searchParams` are `Promise` — must be awaited.

```ts
export default async function Page({
  params,
  searchParams,
}: {
  params: Promise<{ slug: string }>
  searchParams: Promise<{ q?: string }>
}) {
  const { slug } = await params
  const { q } = await searchParams
  // ...
}
```

Same for `cookies()`, `headers()` — all async.

### Use Next.js built-in components

- `next/image` over `<img>` — auto-optimization, lazy loading, sizing
- `next/font` over `<link>` to Google Fonts — no layout shift, self-hosted
- `next/link` over `<a>` — client-side navigation, prefetching

### proxy.ts replaces middleware.ts

In Next.js 16, `middleware.ts` is renamed to `proxy.ts` and the exported function is `proxy()`.
Keep proxy logic lightweight — no DB calls, no JWT verification.
