---
name: spec-driven-setup
description: Set up a new project repository for spec-driven development. Creates the full folder structure (specs/, docs/), configures stack-specific tooling (MCPs, CLAUDE.md, AGENTS.md), and populates project docs from a guided interview. The product-spec, tech-spec, and implement skills are already installed alongside this skill. Use this skill whenever the user says "spec-driven setup", "set up my repo", "initialize project", "start a new project", "set up spec-driven", or wants to scaffold a new codebase with the spec-driven workflow. Also trigger when the user asks to add the spec-driven workflow to an existing repo.
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
AGENTS.md              # Agent-agnostic project conventions (source of truth)
CLAUDE.md              # References AGENTS.md (if using Claude Code)
.mcp.json              # MCP server configurations (committed to repo)

specs/
├── active/            # Current specs being worked on
├── archive/           # Completed feature specs (organized by feature + date)
└── templates/
    ├── prd-template.md
    └── tech-spec-template.md

docs/
├── project-overview.md
├── tech-infra.md
└── design.md
```

Populate each file using the templates in this skill's `templates/` directory. Fill them in with the information gathered during the interview.

### 3. Apply stack-specific configuration

For each framework/platform in the user's stack, read the corresponding file in `specific-stacks/`:
- `nextjs.md` — Next.js (App Router)
- `fastapi.md` — FastAPI (Python)
- `supabase.md` — Supabase (database, auth, realtime)
- `react.md` — React (standalone, e.g., Vite + React)

Each stack file contains:
- Additional setup steps (packages, config files, folder conventions)
- How to configure `tech-infra.md` for that stack
- Agent configuration (`AGENTS.md` entries)
- MCPs to configure in `.mcp.json` (with prompts to ask the user for credentials/project IDs)
- **External agent skills to install** (e.g., `vercel-labs/agent-skills`, `supabase/agent-skills`) via `npx skills add`
- External skills or references to download if available

Apply all relevant stack files. If stacks overlap (e.g., Next.js + Supabase), apply both — they're designed to compose.

**MCP configuration:** Always configure MCPs in the project's `.mcp.json` file so the configuration is committed to the repo and available to all contributors. Do not assume any MCP the agent already has access to is the correct one for this project — always set it up explicitly in `.mcp.json`. The only exception is if `.mcp.json` already exists and contains the exact configuration you would create. Depending on the MCP, ask the user if any extra configuration is needed — for example, how to inject credentials (API keys, tokens, environment variables), whether to use read-only vs read-write mode, or any project-specific connection details.

**Important:** When installing external skills via `npx skills add`, always use non-interactive flags so the command doesn't hang waiting for input. The pattern is:
```bash
npx skills add <repo> --skill <skill-name> --agent <agent-type> --yes
```
Ask the user which agent they're using (e.g., `claude-code`, `cursor`, `codex`, `windsurf`) before running install commands. If unsure, check for config directories (`.claude/`, `.cursor/`, `.codex/`) in the project root.

### 4. Configure the agent

Create or update agent configuration files in the project root:

**AGENTS.md** (agent-agnostic, the single source of truth):
```markdown
# Project: [Name]

## Spec-Driven Workflow
This project uses a spec-driven workflow. Every feature goes through three phases:
1. `/product-spec` — Define what to build (PRD)
2. `/tech-spec` — Design how to build it (technical spec)
3. `/implement` — Build it from both specs

Never skip phases. Product thinking before technical design, technical design before code.

## Key Docs
- `docs/project-overview.md` — Project context and goals
- `docs/tech-infra.md` — Tech stack and architecture
- `docs/design.md` — Design system and guidelines

## Conventions
[Stack-specific conventions from the specific-stacks files]
```

**CLAUDE.md** (if using Claude Code — just references AGENTS.md):
```markdown
Read and follow AGENTS.md for project conventions, workflow, and key docs.
```

If the user is using a different agent tool (Cursor, Windsurf, Codex), adapt accordingly — but `AGENTS.md` should always be the canonical reference.

### 5. Commit the scaffold

After all files are created, commit the scaffolded structure so the user has a clean baseline. Use a message like `"chore: scaffold spec-driven project structure"`. Ask the user before committing.

### 6. Explain the workflow and next steps

After setup is complete, give the user a brief orientation:

> **Your spec-driven workflow is ready.** Here's how it works:
>
> 1. **`/product-spec`** — Start here when you want to build something new. You'll have a conversation about the feature, and the agent will produce a PRD saved to `specs/active/PROD-SPEC-feature-name.md`.
>
> 2. **`/tech-spec`** — Once the PRD is solid, run this to turn it into a technical spec. The agent reads your PRD + project docs, asks clarifying questions, pushes back on bad ideas, and produces a tech spec saved to `specs/active/TECH-SPEC-feature-name.md`.
>
> 3. **`/implement`** — When both specs are approved, run this to build it. The agent reads both specs, creates a plan, and implements incrementally. When done, specs get archived to `specs/archive/`.
>
> **The key rule:** never skip phases. Product thinking before technical design, technical design before code. The specs are your source of truth.

Then suggest next steps:
- "Want to create your first feature spec? Run `/product-spec`."
- "Want to review or tweak any of the docs I generated? I can open them for you."
- "Need to add more stacks or tools later? Just run `/spec-driven-setup` again."

## Rules

- Always interview before scaffolding. Never assume context.
- If this is an existing repo, do NOT overwrite existing files. Ask what to merge or skip.
- The three basic skills (product-spec, tech-spec, implement) are already installed alongside this skill — do not reinstall them.
- Stack-specific steps are additive — they never conflict with each other.
- If a stack the user needs isn't in `specific-stacks/`, set up the generic structure and note what's missing. Don't block the setup.
- Keep the generated docs concise. The user will expand them over time — give them a solid starting point, not an essay.

## Reference files

Read these as needed during setup. All paths are relative to this skill's directory (`spec-driven-setup/`):

| Path | When to read |
|------|-------------|
| `specific-stacks/nextjs.md` | When Next.js is in the stack |
| `specific-stacks/fastapi.md` | When FastAPI is in the stack |
| `specific-stacks/supabase.md` | When Supabase is in the stack |
| `specific-stacks/react.md` | When React (standalone) is in the stack |
| `templates/project-overview-template.md` | When populating `docs/project-overview.md` |
| `templates/tech-infra-template.md` | When populating `docs/tech-infra.md` |
| `templates/design-template.md` | When populating `docs/design.md` |
| `templates/prd-template.md` | When creating `specs/templates/prd-template.md` |
| `templates/tech-spec-template.md` | When creating `specs/templates/tech-spec-template.md` |
