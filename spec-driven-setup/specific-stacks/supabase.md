# Supabase — Stack-Specific Setup

Apply these steps when the project uses Supabase (database, auth, storage, realtime).

## MCP setup

**Always add the Supabase MCP.** Ask the user for their Supabase project ID (the `project_ref` from their Supabase dashboard URL: `supabase.com/dashboard/project/[project_ref]`).

For Claude Code, add to `.mcp.json` or the MCP configuration:
```json
{
  "mcpServers": {
    "supabase": {
      "type": "url",
      "url": "https://mcp.supabase.com/mcp?project_ref=[PROJECT_REF]"
    }
  }
}
```

Ask the user:
- "What's your Supabase project ID? I need it to set up the MCP so the agent can inspect your database schema directly."
- "Should the MCP be read-only or read-write?" (Default to read-write for development projects, read-only for production.)

If using read-only mode, append `&read_only=true` to the URL.

## Additional folder structure

```
src/
├── lib/
│   ├── supabase/
│   │   ├── client.ts      # Browser client (createBrowserClient)
│   │   ├── server.ts      # Server client (createServerClient with cookies)
│   │   ├── admin.ts       # Service role client (server-only, bypasses RLS)
│   │   └── middleware.ts   # Auth session refresh for Next.js middleware
│   └── database.types.ts  # Auto-generated types from Supabase CLI
```

If the project also uses Next.js, these files go inside the existing `src/lib/` structure.

## tech-infra.md additions

Add the following to `docs/tech-infra.md`:

**Database:**
- Supabase (PostgreSQL) — managed via Supabase dashboard and migrations
- Row Level Security (RLS) enabled on all tables by default
- Use `supabase gen types typescript` to generate `database.types.ts`

**Auth:**
- Supabase Auth for user management
- Session handled via cookies (for SSR frameworks) or client-side (for SPAs)
- Always use the server client or middleware to validate sessions on the server
- Never trust client-side auth state for authorization — always verify server-side

**Clients:**
- `createBrowserClient` — for client components and browser-side operations
- `createServerClient` — for server components, server actions, route handlers (uses cookies)
- `createClient` with service role key — for admin operations only, server-side, bypasses RLS. Never expose to the client.

**Conventions:**
- Every table must have RLS policies before being used
- Use identity columns (`id bigint generated always as identity`) for primary keys
- Always add indexes on foreign key columns
- Use RPC functions for complex queries or operations that need to run in a single transaction
- Prefer `select()` with specific columns over `select('*')`
- Handle Supabase errors explicitly — check `.error` on every response

**MCP:**
- Supabase MCP is configured for this project (project ref: `[PROJECT_REF]`)
- Use the MCP to inspect current schema, run queries, and check RLS policies during tech-spec and implementation

## CLAUDE.md / AGENTS.md entries

```markdown
## Supabase Conventions
- RLS is mandatory on all tables. Never create a table without policies.
- Use the server client for server-side operations, browser client for client-side.
- Never expose the service role key to the client.
- Always check .error on Supabase responses.
- Use the Supabase MCP to inspect the database before making changes.
- Generate types after any schema change: supabase gen types typescript.
- Prefer specific column selects over select('*').
- Add indexes on all foreign key columns.
```

## External skills installation

Install Supabase's official agent skills for PostgreSQL best practices. These give the agent access to performance, security, and schema design rules during tech-spec review and implementation.

Determine which agent the user is working with (e.g., `claude-code`, `cursor`, `codex`, `windsurf`). Then run:

```bash
# Install Supabase Postgres best practices
npx skills add supabase/agent-skills \
  --skill postgres-best-practices \
  --agent <agent-type> \
  --yes
```

**Available skills from `supabase/agent-skills`:**

| Skill | What it provides |
|-------|-----------------|
| `postgres-best-practices` | Postgres performance rules across 8 categories: query performance, connection management, schema design, concurrency, RLS, data access patterns, monitoring, advanced features |

After installation, update `docs/tech-infra.md` under "Available Skills" to reference it:

```markdown
### Installed Skills
- `/postgres-best-practices` — Postgres performance and schema optimization rules from Supabase. Consult during tech-spec self-review (especially data model decisions, RLS policies, indexes) and during implementation of any database changes.
```

The tech-spec skill should check this skill when reviewing data model decisions, RLS policy patterns, FK indexing, and query performance. The implement skill should check it before writing any migrations or database queries.

## Initial setup checklist

When scaffolding a project with Supabase:
- [ ] Supabase project created (or existing project ID obtained)
- [ ] MCP configured with the project ref
- [ ] `@supabase/supabase-js` installed
- [ ] If using Next.js: `@supabase/ssr` installed
- [ ] Client files created (`client.ts`, `server.ts`, `admin.ts`)
- [ ] `database.types.ts` generated
- [ ] `.env.local` has `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- [ ] `.env.local` has `SUPABASE_SERVICE_ROLE_KEY` (server-only, not prefixed)
- [ ] Supabase CLI installed if using local development (`npx supabase init`)
- [ ] External agent skills installed (postgres-best-practices)
