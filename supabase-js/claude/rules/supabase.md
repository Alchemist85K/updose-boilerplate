# Supabase Rules

## NEVER expose service_role key to the client

`SUPABASE_SERVICE_ROLE_KEY` is server-side only. It bypasses RLS. Never import or bundle it in client code.

## Client Initialization

Use `@supabase/ssr` for server-side frameworks (Next.js, SvelteKit, etc.).

- Server: `createServerClient()` with cookie handling
- Client: `createBrowserClient()`
- Server Action / Route Handler: `createServerClient()` with cookie access

Never use `createClient()` from `@supabase/supabase-js` directly in server-side frameworks.

## Environment Variables

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=         # server-side only
```

Prefix with `NEXT_PUBLIC_` (Next.js), `VITE_` (Vite), or framework equivalent for client-side access.

## Row Level Security

Enable RLS on tables containing user data. When RLS is disabled, document the reason.
