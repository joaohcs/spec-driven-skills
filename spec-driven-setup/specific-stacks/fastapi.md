# FastAPI — Stack-Specific Setup

Apply these steps when the project uses FastAPI (Python).

## Additional folder structure

```
app/
├── main.py              # FastAPI app instance, CORS, lifespan
├── api/
│   ├── __init__.py
│   └── v1/
│       ├── __init__.py
│       ├── router.py    # Aggregates all route modules
│       └── routes/
│           └── [domain].py
├── core/
│   ├── config.py        # Settings via pydantic-settings (BaseSettings)
│   ├── security.py      # Auth utilities, token validation
│   └── dependencies.py  # Shared FastAPI dependencies (get_db, get_current_user)
├── models/
│   ├── __init__.py
│   └── [domain].py      # SQLAlchemy / Pydantic models
├── schemas/
│   ├── __init__.py
│   └── [domain].py      # Pydantic request/response schemas
├── services/
│   ├── __init__.py
│   └── [domain].py      # Business logic, separated from routes
├── db/
│   ├── session.py       # Database engine and session factory
│   └── migrations/      # Alembic migrations (if not using Supabase)
└── tests/
    ├── conftest.py
    └── api/
        └── test_[domain].py
```

If the project uses Supabase as the database, the `db/` directory may be simplified — Supabase manages migrations and the connection. Use the Supabase Python client (`supabase-py`) or connect directly via `asyncpg`/`psycopg`.

## tech-infra.md additions

Add the following to `docs/tech-infra.md`:

**Framework:**
- FastAPI (Python) — async-first API framework
- Pydantic v2 for request/response validation and settings management
- Uvicorn as the ASGI server

**Project structure:**
- Routes in `app/api/v1/routes/`, grouped by domain
- Business logic in `app/services/` — routes should be thin, delegating to services
- Shared dependencies (auth, DB session) via FastAPI's `Depends()` system
- Configuration via `pydantic-settings` with `.env` file support

**Conventions:**
- All route handlers are `async def` unless they call blocking I/O (use `run_in_executor` or sync `def` in that case)
- Use Pydantic schemas for all request bodies and responses — never pass raw dicts
- Use `HTTPException` for error responses with appropriate status codes
- Use dependency injection for cross-cutting concerns (auth, DB, rate limiting)
- Type all function signatures — FastAPI uses them for automatic OpenAPI docs
- Prefix all API routes with `/api/v1/`

**Testing:**
- `pytest` with `httpx.AsyncClient` for API testing
- Fixtures in `conftest.py` for app instance, test client, test database

**Deployment:**
- Railway, Render, or AWS (depending on project) via Docker
- `Dockerfile` with multi-stage build
- Health check endpoint at `/health`

## CLAUDE.md / AGENTS.md entries

```markdown
## FastAPI Conventions
- All route handlers should be async. Use services for business logic.
- Use Pydantic schemas for all request/response models. Never pass raw dicts.
- Use FastAPI's Depends() for dependency injection (auth, DB session, etc.).
- Type all function signatures — FastAPI generates OpenAPI docs from them.
- Keep routes thin: validate input, call service, return response.
- Use HTTPException with proper status codes for errors.
- Read docs/tech-infra.md before making architectural decisions.
```

## External skills installation

As of now, there is no official `fastapi/agent-skills` repository. If one becomes available, install it the same way:

```bash
# Pattern for when an official repo exists
npx skills add fastapi/agent-skills \
  --agent <agent-type> \
  --yes
```

**If the project also uses Supabase**, the `postgres-best-practices` skill from `supabase/agent-skills` applies — it covers query patterns, schema design, and connection management that are relevant to any Python backend talking to PostgreSQL. See `supabase.md` for installation instructions.

After installing any skills, update `docs/tech-infra.md` under "Available Skills" to reference them so the tech-spec and implement skills know to consult them.

## Python environment setup

Ask the user their preferred Python dependency management:
- **`uv`** (recommended) — fast, modern, handles venv + deps
- **`poetry`** — full-featured, lock files, good for libraries
- **`pip` + `requirements.txt`** — simple, widely understood

Regardless of tool, ensure:
- A virtual environment is created and documented
- A `.python-version` file exists (recommend 3.11+)
- `pyproject.toml` is the single source of truth for project metadata

## Initial setup checklist

When scaffolding a FastAPI project:
- [ ] Python environment initialized (uv/poetry/venv)
- [ ] `fastapi`, `uvicorn`, `pydantic`, `pydantic-settings` installed
- [ ] Project structure created per the folder layout above
- [ ] `app/main.py` with FastAPI app instance, CORS middleware, health endpoint
- [ ] `app/core/config.py` with `BaseSettings` class loading from `.env`
- [ ] `.env` file created with placeholder variables
- [ ] `pyproject.toml` or `requirements.txt` generated
- [ ] If using Supabase: `supabase-py` installed and client configured
- [ ] If using SQLAlchemy: `alembic init` run, migration directory set up
- [ ] Basic `Dockerfile` created (if deployment target requires it)
