# DEVELOPMENT.md

**Planned stack and workflow. Nothing here is installed or configured yet.** Do
not install or scaffold any of this during initialization.

## Stack

### Frontend

```
Next.js
TypeScript
React
Tailwind CSS
React Flow (or an equivalent canvas library — to be confirmed)
```

### Backend

```
Python
FastAPI
```

### Database

```
PostgreSQL
```

### AI

```
External LLM APIs (provider(s) TBD; existing coding-capable models for the
Coding Agent). No self-hosted models, no fine-tuning in V1 (ADR-005).
```

### GitHub

```
GitHub API (connection strategy to be verified against official docs — see
docs/GITHUB.md)
```

### Supporting

```
Docker (where useful, e.g. local Postgres, sandboxed execution)
Simple background job/queue mechanism (where required for agent/validation runs)
```

## Testing

```
Frontend unit / component tests
Backend unit tests
Integration tests (API + database)
Agent workflow tests (Architecture Agent, Coding Agent, end-to-end)
```

Tests are mandatory for meaningful functionality ([CLAUDE.md](../CLAUDE.md)
Rule 8). Completion is not claimed without validation.

## Repository Layout

```
frontend/   Next.js application
backend/    FastAPI application
docs/       Specifications (this directory)
tasks/      Roadmap (TODO.md), the active task (CURRENT_TASK.md), completed/
tests/      Cross-cutting / integration tests
scripts/    Dev and ops scripts
```

Monorepo vs. split repos is an open question ([../PROJECT_MEMORY.md](../PROJECT_MEMORY.md) §30).

## Working Agreement

1. One small, explicit, testable, reviewable, independently-completable task at a
   time.
2. Before implementing: read `PROJECT_MEMORY.md`, `tasks/CURRENT_TASK.md`,
   `tasks/TODO.md`, and relevant `docs/`.
3. Plan → inspect → identify affected files → state approach and risks →
   implement → test → update docs.
4. Keep changes minimal and focused; do not expand scope silently.
5. When a task completes: update `PROJECT_MEMORY.md` (§26–28, §33), check the box
   in `tasks/TODO.md`, move a copy of the task summary into `tasks/completed/`,
   and reset `tasks/CURRENT_TASK.md`.

## Environment

Copy `.env.example` to `.env` and fill in values locally. Never commit `.env`.
