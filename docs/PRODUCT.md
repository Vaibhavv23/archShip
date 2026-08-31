# PRODUCT.md

## Product Vision

Architecture-to-Code is an AI-native software architecture and engineering
platform. Developers design or modify their software architecture on an
interactive canvas that is **semantically architecture-aware** — every component
and connection carries structured meaning (type, technology, protocol, data
flow), not just shape and position.

An AI Architecture Agent reasons about that architecture: it answers questions,
identifies risks, and proposes concrete modifications as structured change plans.
The developer reviews and approves a change. A Coding Agent then implements the
approved change against a connected GitHub repository — inspecting the existing
code, modifying it, running tests and build, fixing issues, and finally opening a
Pull Request.

The long-term goal is for architecture to become the source of truth for code,
databases, APIs, tests, documentation, and infrastructure.

## Core User

Initial focus:

- Software developers
- Technical founders
- Small engineering teams
- Architects
- Teams building SaaS applications

## Core User Problem

Today, architecture diagrams, code, database schemas, API definitions,
documentation, and Git workflows live in different tools that do not talk to each
other:

- Diagrams are decorative and drift from the real system.
- There is no structured, machine-readable model of the architecture.
- Moving from "we should add caching here" to a reviewed code change is entirely
  manual.
- AI coding tools operate on files, not on architecture.

The product connects these: a structured architecture model drives reviewed,
implemented change.

## Core Promise

```
Design → Review → Approve → Implement → PR
```

## V1 User Stories

- As a developer, I can create a software architecture visually.
- As a developer, I can add, move, edit, connect, and delete components on the
  canvas.
- As a developer, I can set the type and technology of each component and the
  protocol of each connection.
- As a developer, I can describe architecture changes to an AI in chat
  (e.g. "Add Redis caching for the product API").
- As a developer, I can see the AI's proposed architecture and a structured
  change plan, grounded in my current architecture.
- As a developer, I can review AI-proposed changes and reject, modify, or approve
  them.
- As a developer, I can approve an architecture change, creating an explicit
  approval record.
- As a developer, I can connect a GitHub repository to a project.
- As a developer, I can ask the Coding Agent to implement an approved change.
- As a developer, I can see validation results (tests, build) from the Coding
  Agent's run.
- As a developer, I can receive a GitHub Pull Request containing the
  implementation, with a description of the architecture and code changes.

## Out of Scope for V1

See [../PROJECT_MEMORY.md](../PROJECT_MEMORY.md) §18 and
[DECISIONS.md](DECISIONS.md) (ADR-009). Notably: no production deployment, no
infrastructure generation, no autonomous unreviewed changes, no
code-to-architecture sync, no drift detection.
