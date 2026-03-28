# Tech Infrastructure

## Stack overview

| Layer | Technology | Notes |
|-------|-----------|-------|
| Frontend | [e.g., Next.js, React + Vite] | |
| Backend | [e.g., FastAPI, Next.js API routes] | |
| Database | [e.g., Supabase/PostgreSQL] | |
| Auth | [e.g., Supabase Auth, NextAuth] | |
| Styling | [e.g., Tailwind CSS + shadcn/ui] | |
| Language(s) | [e.g., TypeScript, Python] | |
| Deployment | [e.g., Vercel, Railway] | |

## Architecture decisions

[Document key decisions and their rationale. Examples:]

### Why [Framework X]?
[Reason — what alternatives were considered, why this was chosen.]

### Why [Database Y]?
[Reason.]

[Add more as needed. This section grows over time as decisions are made.]

## Project structure

```
[Paste or describe the top-level folder structure here]
```

## Environment variables

| Variable | Where used | Description |
|----------|-----------|-------------|
| [VAR_NAME] | [Server/Client/Both] | [What it's for] |

## External services

| Service | Purpose | Auth method |
|---------|---------|------------|
| [e.g., Supabase] | [Database, auth] | [API key] |

## Available tooling

### MCPs
[List any configured MCPs and what they provide. Example:]
- **Supabase MCP** (project ref: `xxxxx`) — inspect database schema, run queries, check RLS policies

### Skills
[List any project-specific skills available to the agent. Example:]
- `/product-spec` — create a PRD for a new feature
- `/tech-spec` — convert a PRD into a technical spec
- `/implement` — implement a feature from its specs

### References
[List any bundled docs or references the agent should consult. Example:]
- Next.js docs in `node_modules/next/dist/docs/`

## Conventions

[Stack-specific conventions go here. These are populated from the specific-stacks files during setup. Examples:]

- Use Server Components by default. Only add "use client" when needed.
- RLS is mandatory on all Supabase tables.
- All API calls go through `src/services/`.

## Development setup

```bash
# How to get the project running locally
[commands here]
```
