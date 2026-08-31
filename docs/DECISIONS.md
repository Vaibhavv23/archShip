# Architecture Decisions

Architecture Decision Records (ADRs). If a decision becomes questionable later,
mark it **Under review** with a note rather than treating it as immutable.

---

## ADR-001 — Modular Monolith for V1

**Status:** Accepted

**Decision:** Use a modular monolith for V1 — a single backend application with
internal modules, not independently deployed services.

**Reason:** The product is already complex at the application level (IR, two agent
layers, approval workflow, GitHub integration). Adding distributed-system
complexity now would slow development with no user benefit.

**Alternatives:** Microservices.

**Why rejected:** Unnecessary operational complexity for V1.

---

## ADR-002 — PostgreSQL as Primary Database

**Status:** Accepted

**Decision:** Use PostgreSQL as the primary application database.

**Reason:** Relational data (users, projects, versions, plans, runs), strong
consistency needs, JSON support for IR snapshots, and broad operational
familiarity.

**Alternatives:** MongoDB; a graph database (Neo4j) for the architecture model.

**Why rejected:** A graph DB adds infrastructure for a model that is small and
fits comfortably in relational + JSON columns. Document DB gives weaker
guarantees for the transactional parts.

---

## ADR-003 — Structured Architecture IR is Canonical

**Status:** Accepted

**Decision:** Use a structured Architecture IR as the canonical architecture
representation, not the canvas/React Flow JSON.

**Reason:** The architecture must be deterministic, serializable, versionable,
extensible, and independent of the UI so it can drive AI reasoning, code,
databases, APIs, and tests. The canvas is a replaceable projection.

**Alternatives:** Treat canvas state as the source of truth.

**Why rejected:** Couples the model to a rendering library and to layout
concerns; hard to version and reason about.

---

## ADR-004 — Human Approval Before Implementation

**Status:** Accepted

**Decision:** Architecture changes require explicit user approval before the
Coding Agent executes.

**Reason:** Implementation edits real repositories. Users must review and
consciously approve architectural change plans.

**Alternatives:** Autonomous application of AI-proposed changes.

**Why rejected:** Unsafe and untrustworthy for a tool that modifies real code.

---

## ADR-005 — Use Existing LLM APIs

**Status:** Accepted

**Decision:** Use existing LLM APIs for both the Architecture Agent and the
Coding Agent.

**Reason:** Building or fine-tuning a foundation model is out of scope, expensive,
and unnecessary to validate the product.

**Alternatives:** Proprietary model, self-hosted open models, fine-tuning.

**Why rejected:** Cost and complexity with no V1 payoff.

---

## ADR-006 — Coding Agent as a Separate Layer

**Status:** Accepted

**Decision:** Keep the Architecture Agent and the Coding Agent conceptually and
architecturally separate, communicating via structured artifacts (IR, change
plan, approval, repo context).

**Reason:** Different responsibilities, different tools, different risk profiles.
Separation enables independent testing and replacement.

**Alternatives:** One agent that reasons about architecture and edits code.

**Why rejected:** Blurs the approval boundary and makes both jobs harder to
reason about.

---

## ADR-007 — GitHub PR as V1 Output

**Status:** Accepted

**Decision:** The Coding Agent creates a Pull Request; it does not push to the
default branch and does not merge its own PR in V1.

**Reason:** Human code review stays in the loop; matches existing team workflows.

**Alternatives:** Direct commit to main; auto-merge on green CI.

**Why rejected:** Too much trust in an unproven system.

---

## ADR-008 — Opinionated V1 Technology Set

**Status:** Accepted

**Decision:** Support a small, fixed set of component types and technologies in
V1 rather than universal framework/cloud coverage.

**Reason:** Focus, quality, and tractable agent behavior. The IR stays
extensible.

**Alternatives:** Broad catalog from the start.

**Why rejected:** Spreads effort thin and makes agent reasoning unreliable.

---

## ADR-009 — No Production Deployment in V1

**Status:** Accepted

**Decision:** V1 ends at a validated GitHub Pull Request. No deployment, no
infrastructure-as-code generation, no production changes.

**Reason:** Deployment is a large, risky problem space orthogonal to the core
architecture-to-code loop.

**Alternatives:** Include deploy/IaC generation.

**Why rejected:** Out of scope for validating the core promise.
