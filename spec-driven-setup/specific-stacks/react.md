# React (Standalone) — Stack-Specific Setup

Apply these steps when the project uses React without Next.js — typically via Vite, or as a standalone SPA.

**Note:** If the project uses Next.js, use `nextjs.md` instead. This file is for standalone React apps (Vite + React, CRA, etc.).

## Additional folder structure

```
src/
├── main.tsx             # Entry point, renders App
├── App.tsx              # Root component, router setup
├── components/
│   ├── ui/              # shadcn/ui primitives (if using shadcn)
│   └── [feature]/       # Feature-specific components
├── pages/               # Route-level components (one per route)
├── hooks/               # Custom React hooks
├── lib/                 # Shared utilities, API clients, helpers
├── types/               # TypeScript type definitions
├── stores/              # State management (if using Zustand, Jotai, etc.)
├── services/            # API call functions, grouped by domain
└── styles/
    └── globals.css      # Tailwind + CSS custom properties
```

## tech-infra.md additions

Add the following to `docs/tech-infra.md`:

**Framework:**
- React with TypeScript (via Vite)
- Client-side SPA — all rendering happens in the browser
- React Router for client-side routing

**Styling:**
- Tailwind CSS for all styling
- shadcn/ui as the component library (if chosen during setup)
- CSS custom properties in `globals.css` for theming
- `cn()` utility for conditional class merging

**State management:**
- React state (`useState`, `useReducer`) for local component state
- [Zustand / Jotai / React Context] for shared state (ask user preference)
- React Query / TanStack Query for server state (API data fetching, caching, sync)

**API communication:**
- All API calls go through `src/services/` — components never call APIs directly
- Use `fetch` or `axios` (ask user preference)
- TanStack Query for data fetching hooks with caching and invalidation

**Conventions:**
- Components are functional with hooks — no class components
- One component per file, named export matching filename
- Custom hooks in `src/hooks/`, prefixed with `use`
- Keep components focused: if it's doing too much, split it
- All props typed with TypeScript interfaces

**Deployment:**
- Static hosting (Vercel, Netlify, Cloudflare Pages, S3 + CloudFront)
- Build output is static HTML/JS/CSS

## CLAUDE.md / AGENTS.md entries

```markdown
## React Conventions
- Functional components with hooks only. No class components.
- Keep components focused — split when they grow beyond a single responsibility.
- API calls go through src/services/, not directly in components.
- Use TanStack Query for server state (data fetching, caching, mutations).
- Local state with useState/useReducer; shared state with [chosen state library].
- All props and function signatures must be typed.
- Read docs/design.md before building any UI.
- Read docs/tech-infra.md before making architectural decisions.
```

## External skills installation

Install Vercel's official agent skills for React best practices and web design. Even though this isn't a Next.js project, the React-specific rules (re-render optimization, bundle size, composition patterns) and web design guidelines still apply.

Determine which agent the user is working with (e.g., `claude-code`, `cursor`, `codex`, `windsurf`). Then run:

```bash
# Install React best practices and web design guidelines
npx skills add vercel-labs/agent-skills \
  --skill react-best-practices \
  --skill web-design-guidelines \
  --skill composition-patterns \
  --agent <agent-type> \
  --yes
```

**Note:** The `react-best-practices` skill includes some Next.js-specific rules — the agent should apply only the React-relevant categories (bundle size, re-render optimization, rendering performance, client-side data fetching) and ignore Next.js-specific ones (Server Components, App Router, middleware).

After installation, update `docs/tech-infra.md` under "Available Skills":

```markdown
### Installed Skills
- `/react-best-practices` — React performance optimization rules. Consult during tech-spec self-review and implementation. Skip Next.js-specific rules (Server Components, App Router).
- `/web-design-guidelines` — UI audit rules for accessibility, forms, animation, typography. Consult during implementation of any UI work.
- `/composition-patterns` — React composition patterns. Consult when designing component APIs.
```

## Initial setup checklist

When scaffolding a standalone React project:
- [ ] `npm create vite@latest` with React + TypeScript template
- [ ] Tailwind CSS installed and configured
- [ ] shadcn/ui initialized (if using — requires Vite alias config)
- [ ] React Router installed and basic route structure created
- [ ] TanStack Query installed and provider set up in `App.tsx`
- [ ] `lib/utils.ts` with `cn()` helper
- [ ] `globals.css` has Tailwind directives and CSS custom properties
- [ ] `.env` created with `VITE_` prefixed variables
- [ ] `tsconfig.json` path aliases configured (`@/` → `src/`)
- [ ] `src/services/` directory with API client base configured
- [ ] External agent skills installed (react-best-practices, web-design-guidelines, composition-patterns)
