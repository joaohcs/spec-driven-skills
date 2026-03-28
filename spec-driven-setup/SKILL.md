---
name: spec-driven-setup
description: Set up a new project repository for spec-driven development. Creates the full folder structure (specs/, docs/), installs the product-spec, tech-spec, and implement skills, configures stack-specific tooling (MCPs, skills, CLAUDE.md), and populates project docs from a guided interview. Use this skill whenever the user says "spec-driven setup", "set up my repo", "initialize project", "start a new project", "set up spec-driven", or wants to scaffold a new codebase with the spec-driven workflow. Also trigger when the user asks to add the spec-driven workflow to an existing repo.
user_invocable: true
---

# Spec-Driven Setup Skill

You are setting up a repository for spec-driven development — a workflow where every feature goes through three phases: product spec → technical spec → implementation. Your job is to interview the user, then scaffold the entire project structure so they can start building immediately.

## Process

### 1. Interview the user

Before creating anything, gather the information you need. Ask these questions conversationally — don't dump them all at once. Group them naturally and adapt based on answers.

**Project context:**
- What is the project? One-line description.
- Who is the target user or customer?
- What problem does it solve? What's the core value proposition?
- Is this a new repo or an existing codebase? (If existing, you'll adapt rather than overwrite.)
- Is there a company or product name?

**Tech stack decisions:**
- What framework(s) will you use? (e.g., Next.js, FastAPI, React standalone)
- What database/backend? (e.g., Supabase, PostgreSQL, Firebase)
- What language(s)? (TypeScript, Python, etc.)
- Hosting/deployment targets? (Vercel, Railway, AWS, etc.)
- Any existing infrastructure or services already in place?
- Auth strategy? (Supabase Auth, NextAuth, custom, etc.)
- Any MCPs, external APIs, or third-party services you already know you'll use?

**Design guidelines:**
- Do you have a design system, brand guidelines, or preferred component library? (Default: shadcn/ui + Tailwind)
- Primary colors, accent colors, or brand palette?
- Font preferences? (Default: Inter for UI, monospace for code)
- Any specific design principles or aesthetic direction? (e.g., minimal, dense, playful)
- Dark mode support needed?

Once you have enough to proceed, confirm a summary with the user before scaffolding.

### 2. Scaffold the folder structure

Create the following structure in the project root:

```
specs/
├── active/           # Current specs being worked on
├── archive/          # Completed feature specs (organized by feature + date)
└── templates/
    ├── prd-template.md
    └── tech-spec-template.md

docs/
├── project-overview.md
├── tech-infra.md
└── design.md
```

Populate each file using the templates in this skill's `templates/` directory. Fill them in with the information gathered during the interview.

### 3. Install the basic skills

Copy the three core skills from this skill's `basic-skills/` directory into the project's skills location:
- `product-spec.md` → the product spec skill
- `tech-spec.md` → the tech spec skill
- `implement.md` → the implement skill

Where to install depends on the project's agent configuration:
- If using **Claude Code** with a `.claude/` directory or `CLAUDE.md`, add them as project skills or reference them in CLAUDE.md.
- If using **Cursor**, **Windsurf**, or another IDE agent, place them where that tool reads custom instructions.
- If the user has a custom setup, ask where skills should go.

### 4. Apply stack-specific configuration

For each framework/platform in the user's stack, read the corresponding file in `specific-stacks/`:
- `nextjs.md` — Next.js (App Router)
- `fastapi.md` — FastAPI (Python)
- `supabase.md` — Supabase (database, auth, realtime)
- `react.md` — React (standalone, e.g., Vite + React)

Each stack file contains:
- Additional setup steps (packages, config files, folder conventions)
- How to configure `tech-infra.md` for that stack
- Agent configuration (CLAUDE.md / AGENTS.md entries)
- MCPs to add (with prompts to ask the user for credentials/project IDs)
- **External agent skills to install** (e.g., `vercel-labs/agent-skills`, `supabase/agent-skills`) via `npx skills add`
- External skills or references to download if available

Apply all relevant stack files. If stacks overlap (e.g., Next.js + Supabase), apply both — they're designed to compose.

**Important:** When installing external skills via `npx skills add`, always use non-interactive flags so the command doesn't hang waiting for input. The pattern is:
```bash
npx skills add <repo> --skill <skill-name> --agent <agent-type> --yes
```
Ask the user which agent they're using (e.g., `claude-code`, `cursor`, `codex`, `windsurf`) before running install commands. If unsure, check for config directories (`.claude/`, `.cursor/`, `.codex/`) in the project root.

### 5. Configure the agent

Based on the stack, create or update the agent configuration file:

**For Claude Code (CLAUDE.md):**
```markdown
# Project: [Name]

## Skills
- /product-spec — Create a PRD for a new feature
- /tech-spec — Convert a PRD into a technical spec
- /implement — Implement a feature from its specs

## Conventions
[Stack-specific conventions from the specific-stacks files]

## Key docs
- docs/project-overview.md — Project context and goals
- docs/tech-infra.md — Tech stack and architecture
- docs/design.md — Design system and guidelines
```

Adapt the format if the user is using a different agent tool.

### 6. Explain the workflow

After setup is complete, give the user a brief orientation:

> **Your spec-driven workflow is ready.** Here's how it works:
>
> 1. **`/product-spec`** — Start here when you want to build something new. You'll have a conversation about the feature, and the agent will produce a PRD (Product Requirements Document) saved to `specs/active/PROD-SPEC-feature-name.md`.
>
> 2. **`/tech-spec`** — Once the PRD is solid, run this to turn it into a technical spec. The agent reads your PRD + project docs, asks clarifying questions, pushes back on bad ideas, and produces a tech spec saved to `specs/active/TECH-SPEC-feature-name.md`.
>
> 3. **`/implement`** — When both specs are approved, run this to build it. The agent reads both specs, creates a plan, and implements incrementally. When done, specs get archived to `specs/archive/`.
>
> **The key rule:** never skip phases. Product thinking before technical design, technical design before code. The specs are your source of truth.

### 7. Offer next steps

Suggest what the user might want to do next:
- "Want to create your first feature spec? Run `/product-spec`."
- "Want to review or tweak any of the docs I generated? I can open them for you."
- "Need to add more stacks or tools later? Just run `/spec-driven-setup` again."

## Rules

- Always interview before scaffolding. Never assume context.
- If this is an existing repo, do NOT overwrite existing files. Ask what to merge or skip.
- The three basic skills (product-spec, tech-spec, implement) must always be installed.
- Stack-specific steps are additive — they never conflict with each other.
- If a stack the user needs isn't in `specific-stacks/`, set up the generic structure and note what's missing. Don't block the setup.
- Keep the generated docs concise. The user will expand them over time — give them a solid starting point, not an essay.

## Reference files

Read these as needed during setup:

| Path | When to read |
|------|-------------|
| `basic-skills/product-spec.md` | When installing the product spec skill |
| `basic-skills/tech-spec.md` | When installing the tech spec skill |
| `basic-skills/implement.md` | When installing the implement skill |
| `specific-stacks/nextjs.md` | When Next.js is in the stack |
| `specific-stacks/fastapi.md` | When FastAPI is in the stack |
| `specific-stacks/supabase.md` | When Supabase is in the stack |
| `specific-stacks/react.md` | When React (standalone) is in the stack |
| `templates/project-overview-template.md` | When populating docs/project-overview.md |
| `templates/tech-infra-template.md` | When populating docs/tech-infra.md |
| `templates/design-template.md` | When populating docs/design.md |
| `templates/prd-template.md` | When creating specs/templates/prd-template.md |
| `templates/tech-spec-template.md` | When creating specs/templates/tech-spec-template.md |
