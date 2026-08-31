# V1 Implementation Roadmap

Work top to bottom, one small task at a time. Do not mark future tasks complete.
Only Phase 0 is done. Each task should be small, explicit, testable, reviewable,
and independently completable.

Legend: `[x]` done · `[ ]` not started

---

## PHASE 0 — Product & Architecture Foundation

- [x] Define product vision, promise, and target users (`docs/PRODUCT.md`)
- [x] Define core product loop and user workflow
- [x] Write system architecture overview (`docs/ARCHITECTURE.md`)
- [x] Specify the Architecture IR (`docs/ARCHITECTURE_IR.md`)
- [x] Specify the AI Architecture Agent (`docs/AI_ARCHITECTURE_AGENT.md`)
- [x] Specify the Coding Agent (`docs/CODING_AGENT.md`)
- [x] Specify GitHub integration (`docs/GITHUB.md`)
- [x] Specify the data model (`docs/DATA_MODEL.md`)
- [x] Specify the security model (`docs/SECURITY.md`)
- [x] Specify the development stack (`docs/DEVELOPMENT.md`)
- [x] Record initial architecture decisions (`docs/DECISIONS.md`, ADR-001…009)
- [x] Create project memory (`PROJECT_MEMORY.md`)
- [x] Create Claude Code instructions (`CLAUDE.md`)
- [x] Create the V1 roadmap (this file)
- [x] Create repository skeleton (`frontend/`, `backend/`, `tasks/`, `tests/`, `scripts/`)

---

## PHASE 1 — Repository & Development Foundation

- [ ] Decide monorepo vs. split repos; record in `docs/DECISIONS.md`
- [ ] Add root tooling config (editorconfig, formatting, linting conventions)
- [ ] Add `Makefile` or task runner for common commands
- [ ] Add local `docker-compose` for PostgreSQL (dev only)
- [ ] Add CONTRIBUTING notes / branch and commit conventions
- [ ] Set up CI skeleton (lint + test on PR)
- [ ] Verify `.env.example` covers all needed variables
- [ ] Document local setup steps in `README.md`

---

## PHASE 2 — Backend Foundation

- [ ] Scaffold FastAPI application in `backend/`
- [ ] Add project structure (modules, config, settings loading from env)
- [ ] Add health check endpoint
- [ ] Add structured logging
- [ ] Add error handling / problem-detail responses
- [ ] Add test harness (pytest) and a first passing test
- [ ] Add database connection layer (no schema yet)
- [ ] Add migration tooling (e.g. Alembic) with an empty baseline
- [ ] Add OpenAPI docs configuration

---

## PHASE 3 — Frontend Foundation

- [ ] Scaffold Next.js + TypeScript app in `frontend/`
- [ ] Add Tailwind CSS
- [ ] Add base layout, routing, and design tokens
- [ ] Add API client module pointing at the backend
- [ ] Add frontend test harness and a first passing test
- [ ] Add linting / type-check scripts
- [ ] Add an empty "Projects" landing page

---

## PHASE 4 — Architecture Data Model

- [ ] Define `projects` table + migration
- [ ] Define `architectures` table + migration
- [ ] Define `components` table + migration
- [ ] Define `connections` table + migration
- [ ] Define enums for component types and technologies (V1 set)
- [ ] Add repository/data-access layer for the above
- [ ] Add model-level tests

---

## PHASE 5 — Architecture IR

- [ ] Define IR schema (typed models) for Project, Component, Connection
- [ ] Add IR serialization (model → JSON)
- [ ] Add IR deserialization (JSON → model) with validation
- [ ] Add deterministic ordering / canonical form
- [ ] Add IR ↔ database mapping (load an architecture as an IR document)
- [ ] Add IR unit tests (round-trip, canonical form, invalid input)
- [ ] Document the concrete IR schema in `docs/ARCHITECTURE_IR.md`

---

## PHASE 6 — Architecture Persistence

- [ ] Define `architecture_versions` table + migration
- [ ] Define `architecture_changes` table + migration
- [ ] Implement "save architecture" (create a new version)
- [ ] Implement "load architecture" (latest or specific version)
- [ ] Implement version diff (compute changes between two versions)
- [ ] Add API endpoints: get / save architecture
- [ ] Add persistence tests

---

## PHASE 7 — Visual Architecture Canvas

- [ ] Create architecture canvas route
- [ ] Integrate canvas library (React Flow or equivalent)
- [ ] Render architecture nodes from the IR
- [ ] Render connections from the IR
- [ ] Add node selection
- [ ] Add node movement
- [ ] Add node deletion
- [ ] Add connection creation
- [ ] Add connection deletion
- [ ] Add zoom
- [ ] Add pan
- [ ] Add save state (canvas → IR → backend)
- [ ] Add load state (backend → IR → canvas)
- [ ] Add canvas component tests

---

## PHASE 8 — Semantic Architecture Components

- [ ] Add component creation UI with type selection (V1 types)
- [ ] Add technology selection per component (V1 technologies)
- [ ] Add component metadata editing (name, description)
- [ ] Add component inspector panel
- [ ] Add architecture grouping / security boundary grouping (visual)
- [ ] Persist component semantics through the IR
- [ ] Tests for component semantics editing

---

## PHASE 9 — Architecture Connections

- [ ] Add connection inspector panel
- [ ] Add protocol selection
- [ ] Add direction (source→target / bidirectional)
- [ ] Add connection metadata (auth, data flow, description)
- [ ] Prevent obviously invalid connections in the UI
- [ ] Persist connection semantics through the IR
- [ ] Tests for connection editing

---

## PHASE 10 — Architecture Validation

- [ ] Implement validation rules: missing target, duplicate IDs, unsupported type/tech
- [ ] Implement constraint checks (e.g. frontend → database directly)
- [ ] Implement circular dependency detection where relevant
- [ ] Add validation API endpoint
- [ ] Surface validation results on the canvas
- [ ] Validation unit tests

---

## PHASE 11 — Project Management

- [ ] Define `users`, `project_members` tables + migrations
- [ ] Implement create / list / open / rename / delete project
- [ ] Implement project membership and roles
- [ ] Add project settings page
- [ ] Enforce project-scoped authorization on all endpoints
- [ ] Project management tests

---

## PHASE 12 — AI Architecture Assistant

- [ ] Add LLM client layer (provider-agnostic interface)
- [ ] Define the Architecture Agent prompt / context builder (from IR)
- [ ] Implement read-only agent: answer architecture questions
- [ ] Implement architecture explanation and risk identification
- [ ] Add AI chat UI panel bound to a project
- [ ] Ground responses in the current IR; reject inconsistent output
- [ ] Agent tests with fixture architectures

---

## PHASE 13 — Architecture Change Planning

- [ ] Define the Change Plan schema (operations + implications)
- [ ] Define `change_plans` persistence + migration
- [ ] Implement agent tools: create/update/delete component, create/delete connection (working copy)
- [ ] Implement `generate_change_plan()`
- [ ] Compute application changes, environment vars, and required tests from the plan
- [ ] Render the proposed architecture as a canvas diff
- [ ] Change planning tests (e.g. "Add Redis" fixture)

---

## PHASE 14 — Human Approval Workflow

- [ ] Define `approvals` table + migration
- [ ] Add change plan review UI (side-by-side / diff view)
- [ ] Implement approve / reject / request-modification actions
- [ ] Record approvals with user, timestamp, exact plan
- [ ] Block downstream implementation unless an approval exists
- [ ] Approval workflow tests

---

## PHASE 15 — GitHub Authentication / Repository Connection

- [ ] Verify GitHub connection strategy against official docs; record decision
- [ ] Implement GitHub OAuth / App connection flow (server-side)
- [ ] Define `repositories`, `repository_connections` tables + migrations
- [ ] Store credentials securely (secret manager / encrypted)
- [ ] Create repository connection flow (UI)
- [ ] List accessible repositories
- [ ] Verify repository access
- [ ] Connection tests (mocked GitHub)

---

## PHASE 16 — Repository Analysis

- [ ] Define the repository context model
- [ ] Implement repository checkout into the execution environment
- [ ] Detect language(s), package manager, test command, build command
- [ ] Implement file listing and reading with size/– limits
- [ ] Implement code search
- [ ] Summarize repository structure for the agent
- [ ] Repository analysis tests on sample repos

---

## PHASE 17 — Coding Agent Foundation

- [ ] Define the agent state machine (Understand → Plan → Modify → Test → Fix → Validate → Commit → PR)
- [ ] Define `agent_runs`, `agent_actions` tables + migrations
- [ ] Implement run lifecycle (start, step, record action, finish, fail)
- [ ] Implement the implementation-plan step (LLM, grounded in IR + change plan + repo context)
- [ ] Implement retry limits and termination conditions
- [ ] Foundation tests (state transitions, action logging)

---

## PHASE 18 — Coding Agent Tools

- [ ] Implement `read_file()`
- [ ] Implement `search_code()`
- [ ] Implement `list_files()`
- [ ] Implement `create_file()`
- [ ] Implement `modify_file()`
- [ ] Implement `delete_file()`
- [ ] Implement `run_command()` (sandboxed)
- [ ] Implement `run_tests()`
- [ ] Implement `run_build()`
- [ ] Implement `run_linter()`
- [ ] Implement `create_branch()`
- [ ] Implement `git_diff()`
- [ ] Implement `git_commit()`
- [ ] Implement diff generation for review
- [ ] Enforce tool safety boundaries (scope, secrets, branch protection)
- [ ] Per-tool tests

---

## PHASE 19 — Architecture-to-Code Implementation

- [ ] Wire approved change plan + IR + repo context into a Coding Agent run
- [ ] Implement the modify-code loop using the tools
- [ ] Ensure the agent inspects relevant code before editing
- [ ] Prevent full-app regeneration; require focused changes
- [ ] Produce a change summary (files touched, rationale)
- [ ] Implementation tests on a sample repo ("Add Redis" end to end, no PR)

---

## PHASE 20 — Validation / Tests / Build

- [ ] Define `validation_runs` table + migration
- [ ] Run tests and capture structured results
- [ ] Run build and capture structured results
- [ ] Run linter and capture results
- [ ] Implement failure analysis feeding the fix loop
- [ ] Enforce a maximum number of fix iterations
- [ ] Validation tests

---

## PHASE 21 — Git Branch & Commit

- [ ] Create a feature branch with a deterministic name
- [ ] Stage agent changes
- [ ] Generate a descriptive commit message from the change plan
- [ ] Create the commit
- [ ] Handle no-op / empty-diff cases
- [ ] Branch & commit tests

---

## PHASE 22 — GitHub Pull Request Creation

- [ ] Verify PR creation API against official docs
- [ ] Generate PR title from the change plan
- [ ] Generate structured PR description (summary, architecture changes, implementation changes, database changes, API changes, tests, validation results, architecture impact)
- [ ] Create the PR (never merge)
- [ ] Attach validation results
- [ ] Define `pull_requests` table + migration; persist PR metadata and status
- [ ] Display the PR URL in the UI
- [ ] PR creation tests (mocked GitHub)

---

## PHASE 23 — End-to-End Agent Workflow

- [ ] Wire the full loop: architecture change → plan → approval → coding agent → validation → branch → commit → PR
- [ ] Add a run status UI (steps, actions, logs, result)
- [ ] Handle and surface failures at each stage
- [ ] End-to-end test on a sample repository

---

## PHASE 24 — Agent Reliability & Guardrails

- [ ] Add input/output schema validation for all LLM calls
- [ ] Add guardrails against out-of-scope file changes
- [ ] Add guardrails against secret exposure in diffs/commits
- [ ] Add timeouts and resource limits per run
- [ ] Add graceful cancellation
- [ ] Add idempotency / resume for interrupted runs
- [ ] Reliability tests (failure injection)

---

## PHASE 25 — Security

- [ ] Implement authentication end to end
- [ ] Enforce the authorization chain (user → project → architecture → approval → agent → repo)
- [ ] Harden the sandboxed execution environment (isolation, egress limits)
- [ ] Encrypt secrets at rest; verify none are logged
- [ ] Make repository checkouts ephemeral
- [ ] Add audit log views
- [ ] Security review of agent tool permissions
- [ ] Document LLM data boundaries and provider terms

---

## PHASE 26 — Observability

- [ ] Structured logs across backend and agents
- [ ] Agent run traces (per-action timing and outcome)
- [ ] Metrics: agent success rate, run duration, validation pass rate, PR creation rate
- [ ] Error tracking integration
- [ ] Basic dashboards
- [ ] Alerting on repeated agent failures

---

## PHASE 27 — V1 UX Polish

- [ ] Canvas usability pass (snapping, alignment, keyboard shortcuts)
- [ ] Change plan review clarity
- [ ] Agent run progress and error messaging
- [ ] Empty states and onboarding for a first project
- [ ] Loading / error / retry states across the app
- [ ] Accessibility pass

---

## PHASE 28 — Evaluation

- [ ] Build a set of reference architectures and change requests
- [ ] Define success criteria per scenario (plan quality, implementation correctness, PR quality)
- [ ] Automated eval harness for the Architecture Agent
- [ ] Automated eval harness for the Coding Agent (against sample repos)
- [ ] Track eval results over time

---

## PHASE 29 — V1 Beta

- [ ] Onboarding flow for beta users
- [ ] Usage limits / quotas
- [ ] Feedback capture in-product
- [ ] Known-limitations document for beta users
- [ ] Support / incident process
- [ ] Beta with a small set of real repositories

---

## PHASE 30 — V1 Definition of Done

- [ ] A user can create/modify an architecture manually and via AI
- [ ] AI proposes a structured change plan grounded in the IR
- [ ] The user can review and approve the plan
- [ ] The user can connect a GitHub repository
- [ ] The Coding Agent implements the approved change in that repository
- [ ] Tests and build run and results are captured
- [ ] A branch and commit are created
- [ ] A GitHub Pull Request is created with structured metadata
- [ ] The PR URL is shown to the user
- [ ] Security, audit logging, and sandboxing are in place
- [ ] Evaluation scenarios pass at the agreed threshold
- [ ] Documentation (`PROJECT_MEMORY.md`, `docs/`) reflects the shipped system
