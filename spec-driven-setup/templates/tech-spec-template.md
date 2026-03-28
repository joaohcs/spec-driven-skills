# Tech Spec: [Feature Name]

**Status:** Draft | In Review | Approved
**Author:** [Name]
**Date:** [YYYY-MM-DD]
**PRD:** `specs/active/PROD-SPEC-[feature-name].md`

## Overview

[What this implements, in 2-3 sentences. Link back to the PRD's goal.]

## Technical Approach

[High-level architecture decisions, patterns chosen, and rationale. Why this approach over alternatives?]

## Data Model

[New or modified tables, types, RPC functions, access control policies.]

```sql
-- Example: new table
CREATE TABLE [table_name] (
  id bigint generated always as identity primary key,
  ...
);

-- RLS policies
ALTER TABLE [table_name] ENABLE ROW LEVEL SECURITY;
CREATE POLICY "..." ON [table_name] FOR SELECT USING (...);
```

[If no data model changes, state "No data model changes required."]

## API / Endpoints

| Method | Route | Description | Auth required |
|--------|-------|-------------|---------------|
| [GET/POST/etc.] | [/api/v1/...] | [What it does] | [Yes/No] |

**Request/response shapes:**

```typescript
// [Endpoint name]
// Request
{ ... }

// Response
{ ... }

// Error
{ error: string, code: string }
```

[If using Server Actions instead of API routes, document them here with the same level of detail.]

## Components / Pages

[UI structure, key components, state management approach.]

| Component | Location | Description |
|-----------|----------|-------------|
| [ComponentName] | `src/components/[path]` | [What it renders, key behavior] |

[If this is a backend-only feature, state "No UI changes."]

## Dependencies

| Dependency | Type | Purpose |
|-----------|------|---------|
| [Package name] | npm/pip package | [Why it's needed] |
| [Service name] | External service | [What it provides] |
| [ENV_VAR] | Environment variable | [What it configures] |

## Out of Scope

- [Technical work explicitly deferred]
- [Related improvements that belong in a separate spec]

## Acceptance Criteria

- [ ] [Testable condition from PRD, now with technical specificity]
- [ ] [Additional technical criteria: performance, security, edge cases]

## Open Questions

- [Any remaining technical uncertainties]
