# Architecture-to-Code

## What is this?

An AI-native software architecture and engineering platform. Developers design or
modify software architecture on an interactive, semantically-aware canvas, use an
AI Architecture Agent to reason about and propose changes, review and approve
those changes, and then have a Coding Agent implement the approved changes in a
real codebase and open a GitHub Pull Request.

## Product Vision

> Design or change your software architecture visually or with AI. Review the
> proposed changes. Approve them. Our coding agent turns the approved
> architecture into real code and creates the Pull Request.

Long term:

> Architecture becomes the source of truth for code, databases, APIs, tests,
> documentation, and infrastructure.

## Core Workflow

```
Architecture
  → AI
  → Approval
  → Coding Agent
  → Tests
  → GitHub PR
```

## V1

V1 focuses on one narrow, high-value workflow: create/modify architecture
(manually or via AI chat) → generate a structured change plan → human approval →
connect a GitHub repo → Coding Agent implements → validation → branch, commit,
Pull Request.

V1 ends at a validated GitHub Pull Request. No production deployment, no
infrastructure generation, no autonomous changes. See
[docs/PRODUCT.md](docs/PRODUCT.md) and [tasks/TODO.md](tasks/TODO.md).

## Architecture

Modular monolith for V1. Next.js + TypeScript frontend, FastAPI + Python backend,
PostgreSQL, external LLM APIs, GitHub API. The canvas (presentation) is kept
separate from the Architecture IR (structured source of truth), which is kept
separate from the AI Architecture Agent, the Change Plan, and the Coding Agent.
See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Repository Structure

```
.
├── CLAUDE.md              Instructions for Claude Code sessions
├── PROJECT_MEMORY.md      Canonical project memory / state
├── README.md
├── .gitignore
├── .env.example
├── frontend/              Next.js app (not yet implemented)
├── backend/               FastAPI app (not yet implemented)
├── docs/                  Product & engineering specifications
├── tasks/                 Roadmap, current task, completed tasks
├── tests/                 Cross-cutting / integration tests
└── scripts/               Dev & ops scripts
```

## Development Workflow

Work proceeds one small, testable, reviewable task at a time along the roadmap in
[tasks/TODO.md](tasks/TODO.md). Before implementing, read
[PROJECT_MEMORY.md](PROJECT_MEMORY.md), [tasks/CURRENT_TASK.md](tasks/CURRENT_TASK.md),
and the relevant `docs/`. See [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md).

## Documentation

- [docs/PRODUCT.md](docs/PRODUCT.md) — product vision, users, user stories
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — system architecture
- [docs/ARCHITECTURE_IR.md](docs/ARCHITECTURE_IR.md) — Architecture IR spec
- [docs/AI_ARCHITECTURE_AGENT.md](docs/AI_ARCHITECTURE_AGENT.md) — Architecture Agent
- [docs/CODING_AGENT.md](docs/CODING_AGENT.md) — Coding Agent
- [docs/GITHUB.md](docs/GITHUB.md) — GitHub integration
- [docs/DATA_MODEL.md](docs/DATA_MODEL.md) — planned data model
- [docs/SECURITY.md](docs/SECURITY.md) — security model
- [docs/DEVELOPMENT.md](docs/DEVELOPMENT.md) — stack & workflow
- [docs/DECISIONS.md](docs/DECISIONS.md) — architecture decision records

## Current Status

Status: Project initialization

No product functionality has been implemented. Only documentation, project
structure, and the roadmap exist.
