# Next.js — Stack-Specific Setup

Apply these steps when the project uses Next.js (App Router).

## Additional folder structure

Ensure the project follows the App Router conventions:

```
src/
├── app/                # App Router pages and layouts
│   ├── layout.tsx
│   ├── page.tsx
│   └── (routes)/
├── components/
│   ├── ui/             # shadcn/ui primitives (auto-managed)
│   └── [feature]/      # Feature-specific components
├── lib/                # Shared utilities, clients, helpers
├── hooks/              # Custom React hooks
├── types/              # TypeScript type definitions
└── styles/
    └── globals.css     # Tailwind + CSS custom properties
```

## tech-infra.md additions

Add the following to `docs/tech-infra.md` under the relevant sections:

**Framework:**
- Next.js (App Router) with TypeScript
- React Server Components by default; use `"use client"` only when needed (interactivity, hooks, browser APIs)
- Server Actions for mutations — prefer over API route handlers where possible
- Middleware for auth checks and redirects

**Styling:**
- Tailwind CSS for all styling
- shadcn/ui as the component library (components live in `src/components/ui/`)
- CSS custom properties in `globals.css` for theming (colors, radii, spacing)
- `cn()` utility from `lib/utils.ts` for conditional class merging

**Conventions:**
- File-based routing via the `app/` directory
- Collocate loading.tsx, error.tsx, and not-found.tsx with route segments
- Use `next/image` for all images, `next/link` for navigation
- Environment variables: `NEXT_PUBLIC_` prefix for client-side, server-only otherwise
- Prefer `fetch` with Next.js caching semantics over third-party HTTP clients

**Deployment:**
- Vercel (default) — configure via `vercel.json` if needed
- Environment variables managed in Vercel dashboard

## CLAUDE.md / AGENTS.md entries

Add these conventions to the agent configuration:

```markdown
## Next.js Conventions
- Use App Router. Do not use Pages Router patterns.
- Default to Server Components. Only add "use client" when the component needs interactivity, hooks, or browser APIs.
- Use Server Actions for data mutations. Prefer them over API route handlers.
- Always handle loading, error, and empty states for async operations.
- Use next/image for images, next/link for navigation.
- Follow the component structure: ui/ for primitives, feature folders for domain components.
- Read docs/design.md before building any UI.
- Read docs/tech-infra.md before making architectural decisions.
```

## External skills installation

Install Vercel's official agent skills for React and Next.js best practices. These give the agent access to 40+ performance and design rules during tech-spec review and implementation.

Determine which agent the user is working with (e.g., `claude-code`, `cursor`, `codex`, `windsurf`). Then run the following commands non-interactively:

```bash
# Install React/Next.js best practices and web design guidelines
npx skills add vercel-labs/agent-skills \
  --skill react-best-practices \
  --skill web-design-guidelines \
  --skill composition-patterns \
  --agent <agent-type> \
  --yes
```

**Available skills from `vercel-labs/agent-skills`:**

| Skill | What it provides |
|-------|-----------------|
| `react-best-practices` | 40+ React/Next.js performance rules across 8 categories (waterfalls, bundle size, SSR, re-renders) |
| `web-design-guidelines` | 100+ UI audit rules covering accessibility, forms, animation, typography, dark mode |
| `composition-patterns` | React composition patterns — compound components, state lifting, avoiding prop drilling |
| `react-native-guidelines` | Only install if the project uses React Native / Expo |
| `vercel-deploy-claimable` | Only install if deploying via Vercel from Claude Desktop or claude.ai |

Ask the user which skills they want. Default to installing `react-best-practices`, `web-design-guidelines`, and `composition-patterns` unless they say otherwise.

After installation, update `docs/tech-infra.md` under "Available Skills" to reference the installed skills so the tech-spec and implement skills know to consult them:

```markdown
### Installed Skills
- `/react-best-practices` — React/Next.js performance optimization rules. Consult during tech-spec self-review and implementation.
- `/web-design-guidelines` — UI audit rules for accessibility, forms, animation, typography. Consult during implementation of any UI work.
- `/composition-patterns` — React composition patterns. Consult when designing component APIs.
```

## Additional references

If the project has Next.js docs bundled (e.g., in `node_modules/next/dist/docs/`), reference them in `tech-infra.md` so the agent knows to check them for API signatures, file conventions, and breaking changes.

## Initial setup checklist

When scaffolding a new Next.js project, ensure:
- [ ] `npx create-next-app@latest` with TypeScript, Tailwind, App Router, `src/` directory
- [ ] shadcn/ui initialized (`npx shadcn@latest init`)
- [ ] `lib/utils.ts` exists with the `cn()` helper
- [ ] `globals.css` has Tailwind directives and CSS custom properties for the design system
- [ ] `.env.local` created with placeholder variables documented
- [ ] `tsconfig.json` path aliases configured (`@/` → `src/`)
- [ ] External agent skills installed (react-best-practices, web-design-guidelines, composition-patterns)
