---
name: implement
description: Implement a feature based on its PRD and technical spec
user_invocable: true
---

# Implement Skill

You are implementing a feature that has been through product and technical planning. Your goal is to write production-quality code that satisfies both the PRD and tech spec.

## Process

### 1. Load full context
- Read `docs/project-overview.md`, `docs/tech-infra.md`, and `docs/design.md`.
- Read the PRD from `specs/active/PROD-SPEC-[feature-name].md`.
- Read the tech spec from `specs/active/TECH-SPEC-[feature-name].md`.
- If the user doesn't specify which feature, list available specs and ask.
- Read relevant existing code to understand current patterns and conventions.
- Check `docs/tech-infra.md` for MCPs or tooling. If an MCP is available (e.g., Supabase), use it to inspect the current state before making changes.

**Both the PRD and tech spec must exist before implementation begins.** If either is missing, tell the user and suggest running `/product-spec` or `/tech-spec` first.

### 2. Identify questions before coding
Before writing any code, review both specs and flag:
- **Contradictions** between PRD and tech spec.
- **Ambiguities** — anything not specific enough to implement confidently.
- **Blind spots** — untested paths, missing error handling, undefined behavior.
- **Sequencing concerns** — dependencies between tasks, migrations that must run first.

Ask all questions upfront. Do not start coding with unresolved ambiguity.

### 3. Build an implementation plan
Present a step-by-step plan to the user before coding. The plan should:
- Break the work into small, logical increments.
- Order tasks by dependency (e.g., data model before API before UI).
- Identify what can be tested at each step.
- Call out any deviations from the tech spec and why.

Wait for the user to approve the plan before starting.

### 4. Implement incrementally
- Follow the approved plan step by step.
- After each meaningful step, briefly state what was done and what's next.
- If you hit an unexpected issue, pause and discuss with the user rather than improvising a workaround.
- Run the dev server or tests when appropriate to verify work.
- Follow the design system in `docs/design.md` for all UI work.
- Follow the conventions and patterns documented in `docs/tech-infra.md`.

### 5. Verify against acceptance criteria
When implementation is complete:
- Walk through every acceptance criterion from both the PRD and tech spec.
- Confirm each is met, or flag any that are not and explain why.
- Suggest manual testing steps the user should perform.

### 6. Archive specs

When the user confirms the feature is done:

1. Create a folder in `specs/archive/` named `[feature-name]-YYYY-MM-DD/` using the date the feature was concluded.
2. Move the PRD and tech spec from `specs/active/` into this folder.
3. Create a `summary.md` file in the same folder with a brief overview:
   - What was built (1-2 sentences)
   - Key technical decisions made during implementation
   - Any deviations from the original specs and why
   - Files and routes created or modified
   - Database or infrastructure changes applied

## Rules
- **Never start coding without reading both the PRD and tech spec.**
- Follow existing codebase patterns. Do not introduce new patterns without discussing first.
- Do not add features, refactoring, or "improvements" beyond what the specs define.
- Keep the user informed of progress but don't over-explain — be concise.
- If the specs are wrong or incomplete, raise it. Do not silently deviate.
- Use the naming convention: `PROD-SPEC-[feature-name].md` and `TECH-SPEC-[feature-name].md`.
- Check `docs/tech-infra.md` for stack-specific tools, skills, and conventions before writing code.
