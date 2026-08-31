# ARCHITECTURE.md

**This is an initial conceptual architecture. It will evolve as the product is
built. Changes to core architecture must be reflected here and in
[DECISIONS.md](DECISIONS.md).**

## System Overview

```
                         Next.js
                            │
                            ▼
                    Architecture Canvas
                            │
                            ▼
                    Architecture API
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
      Architecture IR   AI Agent       Projects
             │              │
             │              ▼
             │        Change Plan
             │              │
             └──────────────┼──────────────┐
                            ▼              │
                       Approval            │
                            │              │
                            ▼              │
                      Coding Agent         │
                            │              │
                            ▼              │
                      Git Repository ◄─────┘
                            │
                            ▼
                         Tests
                            │
                            ▼
                       GitHub PR
```

## Deployment Shape

Modular monolith for V1 (ADR-001). A single backend application with internal
modules rather than a set of independently deployed services. Background jobs
(agent runs, validation) run as simple workers/queues, not distributed
infrastructure.

## Layers

### Frontend

- Next.js + TypeScript + React + Tailwind CSS.
- The **Architecture Canvas**: a pan/zoom canvas (React Flow or equivalent) that
  renders components and connections and lets the user edit them.
- An **AI chat** panel for talking to the Architecture Agent.
- A **review & approval** UI for change plans.
- A **repository & PR** UI for connecting GitHub and viewing agent runs.
- The frontend renders and edits; it does not own the canonical architecture
  model.

### Backend

- Python + FastAPI.
- Exposes the **Architecture API**: CRUD for projects, architectures, components,
  connections; serialization to/from the Architecture IR; validation.
- Orchestrates agent runs (Architecture Agent, Coding Agent) and validation runs.
- Owns persistence and all secrets.

### Architecture Model / IR

- The structured, canonical representation of a project's architecture
  (ADR-003). Deterministic, serializable, versionable, extensible, independent of
  the canvas.
- The canvas is a *projection* of the IR; edits are applied to the IR.
- Full spec: [ARCHITECTURE_IR.md](ARCHITECTURE_IR.md).

### AI Layer — Architecture Agent

- Consumes the current IR + user request + project context + constraints.
- Produces an explanation, a proposed IR, a structured Change Plan, and risks.
- Uses existing LLM APIs (ADR-005). Proposes only — never triggers implementation
  (ADR-004, Rule 6).
- Spec: [AI_ARCHITECTURE_AGENT.md](AI_ARCHITECTURE_AGENT.md).

### Change Plan + Approval

- The Change Plan is a structured diff of the architecture plus derived
  application/environment/testing implications.
- An Approval is an explicit, recorded user decision. Only an approved plan may
  be dispatched to the Coding Agent.

### Coding Layer — Coding Agent

- Consumes the IR + approved Change Plan + repository context + constraints.
- Loop: `Understand → Plan → Modify → Test → Fix → Validate → Commit → PR`.
- Bounded toolset (filesystem, git, terminal, testing, GitHub), retry limits,
  inspects before editing (ADR-006, Rule 7).
- Spec: [CODING_AGENT.md](CODING_AGENT.md).

### Repository Layer

- Connects to user-authorized GitHub repositories.
- Provides repository discovery, cloning/checkout, file access, branch/commit
  operations for the Coding Agent.
- Requires a secure, isolated execution environment for running arbitrary
  repository commands (see [SECURITY.md](SECURITY.md)).

### GitHub

- OAuth-based repository connection; branch creation; commits; Pull Request
  creation with structured metadata. V1 output is a PR, never a direct merge
  (ADR-007).
- Spec: [GITHUB.md](GITHUB.md).

### Persistence

- PostgreSQL (ADR-002). Stores users, projects, architectures and versions,
  components, connections, change plans, approvals, repository connections, agent
  runs and actions, validation runs, pull requests.
- Spec: [DATA_MODEL.md](DATA_MODEL.md).

### Security

- Authorization chain: `User → Project → Architecture → Approved Change → Agent
  Permissions → Repository`.
- Secret storage server-side only; isolated execution for agent tools; audit
  logging of agent actions. Spec: [SECURITY.md](SECURITY.md).

### Observability

- Structured logs, agent run traces (each tool call recorded as an agent action),
  validation run records, and metrics for agent success/failure and latency.
  Detailed design deferred; see [tasks/TODO.md](../tasks/TODO.md) Phase 26.

## Key Separation Principle

`Canvas` ⟂ `Architecture Model/IR` ⟂ `Architecture Agent` ⟂ `Change Plan` ⟂
`Coding Agent`. These are not tightly coupled. Each communicates through
well-defined data structures (IR documents, change plans, approvals, repository
context) so that any one can be replaced or tested in isolation.
