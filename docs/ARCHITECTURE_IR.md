# ARCHITECTURE_IR.md

**Conceptual specification only. Not implemented. The Architecture IR is one of
the most important foundations of the entire system (see [CLAUDE.md](../CLAUDE.md)
Rule 5, [DECISIONS.md](DECISIONS.md) ADR-003).**

## Purpose

The Architecture IR (Intermediate Representation) is the structured, canonical
model of a project's software architecture. It exists so that:

- The architecture has a single, machine-readable source of truth.
- AI reasoning operates on structured semantics, not on pixels or free text.
- Changes can be expressed as deterministic, reviewable diffs (Change Plans).
- The same architecture can drive code, databases, APIs, tests, and eventually
  infrastructure.
- The canvas is a replaceable rendering layer, not the model itself.

Required properties: **structured, deterministic, serializable, versionable,
extensible, UI-independent.**

## Relationship to the Canvas

The canvas renders a *projection* of the IR (layout positions, grouping, visual
hints may be stored alongside but are not the architecture). User edits and AI
edits are both applied to the IR. Two different canvases could render the same IR.

## Core Entities

```
Project
 ├── Components
 ├── Technologies
 ├── Interfaces
 ├── Connections
 ├── Protocols
 ├── Data stores
 ├── APIs
 ├── Dependencies
 ├── Constraints
 ├── Environment information
 └── Security boundaries
```

Entity list:

- **Project** — name, description, metadata, IR version.
- **Component** — a unit of the system (see component types below).
- **Connection** — a directed or bidirectional relationship between components.
- **Interface** — a named entry point a component exposes (e.g. an HTTP API, a
  queue consumer).
- **Technology** — a concrete technology bound to a component or interface
  (e.g. `postgresql`, `nextjs`).
- **DataStore** — a component or sub-concept that persists data; may carry schema
  hints.
- **API** — a described interface surface (endpoints, methods) exposed by a
  component.
- **Constraint** — a rule the architecture must satisfy (e.g. "frontend must not
  talk directly to the database").
- **Environment** — environment-level information (e.g. required environment
  variables, regions, runtime).
- **SecurityBoundary** — a grouping that marks a trust boundary between sets of
  components.

## Component Types (V1)

```
frontend
backend
service
database
cache
queue
worker
external_api
authentication
storage
```

The set is extensible; V1 stays opinionated and limited (ADR-008).

## Technologies (V1)

```
nextjs
react
nodejs
fastapi
postgresql
mongodb
redis
```

## Connection Properties

Potential properties:

```
source          component id
target          component id
protocol         e.g. https, postgresql, redis, amqp, grpc
direction        source→target | bidirectional
authentication   how the connection is authenticated
data_flow        what data moves and which way
description      human-readable note
```

## Conceptual Example

```json
{
  "project": { "name": "Example SaaS" },
  "components": [
    { "id": "frontend", "type": "frontend", "technology": "nextjs" },
    { "id": "backend",  "type": "backend",  "technology": "fastapi" },
    { "id": "database", "type": "database", "technology": "postgresql" }
  ],
  "connections": [
    { "source": "frontend", "target": "backend",  "protocol": "https" },
    { "source": "backend",  "target": "database", "protocol": "postgresql" }
  ]
}
```

Component-level example of the semantic intent:

```json
{ "id": "postgres", "type": "database", "technology": "postgresql" }
```

```json
{ "source": "backend", "target": "postgres", "protocol": "postgresql" }
```

## Versioning

Architecture representations must be versionable. Each meaningful change produces
a new version (an immutable snapshot and/or an ordered set of applied changes),
so that history can be inspected, diffed, and rolled back, and so that a Change
Plan references a specific base version. The exact storage strategy (full
snapshots vs. change log vs. hybrid) is an open question — see
[../PROJECT_MEMORY.md](../PROJECT_MEMORY.md) §30.

## Validation (future)

The model should eventually support validation such as:

```
invalid connection (source or target does not exist)
missing target
duplicate IDs
unsupported component type or technology
circular dependency where not allowed
missing required metadata
constraint violation (e.g. frontend → database directly)
```

Validation results feed both the canvas (surface warnings) and the Architecture
Agent (reason about problems). **Not implemented.**
