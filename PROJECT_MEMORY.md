# Project Memory

Canonical, always-current record of what this project is and where it stands.
Update this file whenever project state, scope, decisions, or the current task
change.

## 1. Project Name

Architecture-to-Code (working name; repo: `archShip`).

## 2. One-Line Description

An AI-native platform where developers design or modify software architecture
visually or with AI, approve the proposed changes, and have a Coding Agent
implement them in a real codebase and open a GitHub Pull Request.

## 3. Product Vision

Architecture becomes the source of truth for code, databases, APIs, tests,
documentation, and infrastructure. V1 delivers the first slice of that:
architecture → AI reasoning → human approval → implementation → validation →
GitHub PR.

## 4. Core Problem

Developers keep architecture diagrams, code, database schemas, API definitions,
documentation, and Git workflows in separate, disconnected tools. Diagrams drift
from reality and carry no semantic meaning. There is no reliable path from an
architectural decision to a reviewed code change.

## 5. Product Promise

> Design or change your software architecture visually or with AI. Review the
> proposed changes. Approve them. Our coding agent turns the approved
> architecture into real code and creates the Pull Request.

## 6. Target Users

Software developers, technical founders, small engineering teams, architects, and
teams building SaaS applications.

## 7. Primary User Workflow

1. Create architecture on the canvas.
2. Modify it manually or via AI chat.
3. Review the architecture and the proposed change.
4. Generate an Architecture Change Plan.
5. Approve the plan.
6. Connect a GitHub repository.
7. The Coding Agent implements the approved change.
8. Tests / build / validation run.
9. A branch and commit are created.
10. A GitHub Pull Request is created.

## 8. Core Product Loop

```
Architecture → AI reasoning → Proposed changes → Human approval →
Implementation → Validation → GitHub PR
```

## 9. Architecture as Source of Truth

A visual box is not just a rectangle; a connection is not just an arrow. Each
carries semantic metadata (type, technology, protocol, direction, etc.). This
structured representation is the Architecture IR, from which AI reasoning, code,
databases, APIs, and tests are eventually derived.

## 10. Architecture IR

A structured, deterministic, serializable, versionable, extensible representation
of a project's architecture, independent of the canvas rendering layer. Core
entities: Project, Component, Connection, Interface, Technology, DataStore, API,
Constraint, Environment, SecurityBoundary. Full spec:
[docs/ARCHITECTURE_IR.md](docs/ARCHITECTURE_IR.md). **Not yet implemented.**

## 11. AI Architecture Agent

Reasons about the architecture: answers questions, identifies problems, suggests
and applies component/connection changes, explains consequences, generates
structured change plans, validates consistency. It **proposes**; it never
triggers repository implementation directly. Spec:
[docs/AI_ARCHITECTURE_AGENT.md](docs/AI_ARCHITECTURE_AGENT.md). **Not yet
implemented.**

## 12. Architecture Change Plans

Every AI modification is represented as structured changes:
`ADD_COMPONENT`, `REMOVE_COMPONENT`, `UPDATE_COMPONENT`, `ADD_CONNECTION`,
`REMOVE_CONNECTION`, `UPDATE_CONNECTION`, `UPDATE_TECHNOLOGY`. A change plan also
describes application changes, environment variables, and required tests.
**Not yet implemented.**

## 13. Human Approval Model

The Architecture Agent proposes; the human reviews and rejects, modifies, or
approves; only an approved plan may be sent to the Coding Agent. V1 is never an
autonomous system that makes unreviewed architectural changes.

## 14. Coding Agent

An implementation layer that turns an approved change plan into repository
changes using existing coding-capable LLM APIs. Conceptual loop:
`Understand → Plan → Modify → Test → Fix → Validate → Commit → PR`. It inspects
existing code before editing and never blindly regenerates the app. Spec:
[docs/CODING_AGENT.md](docs/CODING_AGENT.md). **Not yet implemented.**

## 15. Repository Integration

The Coding Agent receives: Architecture IR + approved Change Plan + repository
context + relevant constraints. It only accesses repositories the user has
explicitly authorized. **Not yet implemented.**

## 16. GitHub PR Workflow

`User approves → create feature branch → analyze repo → implement → run
validation → commit → create Pull Request`. The PR eventually contains title,
summary, architecture changes, implementation changes, database changes, API
changes, tests, validation results, and architecture impact. Spec:
[docs/GITHUB.md](docs/GITHUB.md). **Not yet implemented.**

## 17. V1 Scope

One narrow workflow: create/modify architecture (manual or AI) → review →
generate change plan → approve → connect GitHub repo → Coding Agent implements →
validation → branch, commit, PR. Deliberately limited component types and
technologies. The model is extensible but V1 is opinionated.

## 18. V1 Non-Goals

Autonomous production deployment; Kubernetes/Terraform/IaC generation;
multi-cloud provisioning; production monitoring; performance simulation; cloud
cost optimization; enterprise architecture governance; complex RBAC; multi-agent
swarm; custom foundation model; model fine-tuning; automatic production changes;
universal language/framework/cloud support; full code-to-architecture sync;
architecture drift detection. These are potential future roadmap items.

## 19. Initial Supported Technologies

Component types: frontend, backend/API, service, database, cache, queue, worker,
external_api, authentication, storage.

Technologies: Next.js, React, Node.js, FastAPI, PostgreSQL, MongoDB, Redis.

## 20. High-Level Architecture

Modular monolith. Next.js/TypeScript frontend with a semantically-aware canvas; a
FastAPI/Python backend exposing an Architecture API; PostgreSQL persistence;
external LLM APIs for the Architecture Agent and Coding Agent; GitHub API for
repository integration. Layers are kept loosely coupled:
`Canvas → Architecture Model/IR → Architecture Agent → Change Plan → Approval →
Coding Agent → Repository → Validation → GitHub PR`. See
[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## 21. Major Components

- Architecture Canvas (presentation layer)
- Architecture Model / IR (structured source of truth)
- Architecture API (backend)
- AI Architecture Agent
- Change Plan engine
- Approval workflow
- Coding Agent
- Repository / GitHub integration
- Validation runner
- Persistence (PostgreSQL)

## 22. Data Flow

User edits canvas or chats with the AI → changes are applied to the Architecture
IR → the Architecture Agent produces an explanation + proposed IR + change plan +
risks → human approves → approved plan + IR + repo context go to the Coding Agent
→ Coding Agent edits the repo, runs validation, commits to a branch → GitHub PR
is opened → PR URL surfaced to the user.

## 23. Security Model

`User → Authorization → Project → Architecture → Approved Change → Agent
Permissions → Repository`. The Coding Agent only accesses explicitly authorized
repositories. Terminal/tool execution needs a secure, isolated execution
environment. No secrets in source or logs. See [docs/SECURITY.md](docs/SECURITY.md).

## 24. AI Safety / Control Model

Architecture Agent proposes only. Human approval is a hard boundary before
implementation. Coding Agent has bounded tools, retry limits, and no production
access in V1; it does not merge its own PRs in V1.

## 25. V1 Roadmap

Phases 0–30, from Product & Architecture Foundation through V1 Definition of Done.
Full checklist: [tasks/TODO.md](tasks/TODO.md).

## 26. Current Phase

Phase 0 — Product & Architecture Foundation (documentation) is complete. No
implementation phase has started.

## 27. Current Task

None active. See [tasks/CURRENT_TASK.md](tasks/CURRENT_TASK.md).

## 28. Completed Work

- Project documentation structure
- Claude Code instructions (`CLAUDE.md`)
- Project memory (this file)
- Product specification
- Architecture documentation
- Architecture IR specification
- AI Architecture Agent specification
- Coding Agent specification
- GitHub integration specification
- Data model specification
- Security specification
- Development specification
- Architecture decisions (ADR-001 … ADR-009)
- V1 roadmap

## 29. Known Problems

None yet — no code exists.

## 30. Open Questions

- Which LLM provider(s) for the Architecture Agent vs. the Coding Agent.
- Canvas library: React Flow vs. alternatives.
- Secure execution environment for Coding Agent tools (container, microVM, hosted
  sandbox service).
- Auth provider / strategy for user accounts.
- How Architecture IR versions map to database rows vs. serialized snapshots.
- Monorepo vs. split repos for `frontend/` and `backend/`.

## 31. Architecture Decisions

- ADR-001 — Modular monolith for V1
- ADR-002 — PostgreSQL as primary database
- ADR-003 — Structured Architecture IR (not raw canvas JSON) is canonical
- ADR-004 — Architecture changes require explicit human approval
- ADR-005 — Use existing LLM APIs, not a proprietary model
- ADR-006 — Architecture Agent and Coding Agent are separate layers
- ADR-007 — Coding Agent output is a GitHub PR, not a direct merge
- ADR-008 — Opinionated, limited V1 technology set
- ADR-009 — No production deployment in V1

See [docs/DECISIONS.md](docs/DECISIONS.md).

## 32. Future Vision

- V2: Code → Architecture, Architecture ↔ Code sync, drift detection, AI
  architecture review.
- V3: database / API / test / documentation / infrastructure generation, security
  analysis.
- V4: architecture simulation, performance analysis, cost estimation,
  optimization, deployment generation.
- V5: continuous architecture intelligence, governance, production monitoring,
  automatic drift detection, AI engineering agent.

## 33. Change Log

- 2026-09-01 — Project initialization. Documentation, structure, and roadmap
  created. No product functionality implemented.

---

**Current project state: Project initialization only. No product functionality
has been implemented.**
