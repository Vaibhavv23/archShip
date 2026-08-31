# AI_ARCHITECTURE_AGENT.md

**Conceptual specification only. Not implemented.**

## Role

The AI Architecture Agent reasons about a project's architecture and proposes
changes. It is one of two AI layers in the system and is kept conceptually
separate from the Coding Agent (ADR-006). It uses existing LLM APIs (ADR-005).

## Responsibilities

- Understand the current architecture (from the Architecture IR).
- Answer architecture questions ("Explain this architecture", "Why is this
  risky?").
- Identify potential problems, missing dependencies, and obvious architectural
  violations.
- Suggest components and changes.
- Apply changes to a working copy of the IR: add / remove / modify components,
  connect / disconnect components, update technologies.
- Explain architectural consequences of a change.
- Generate a structured Architecture Change Plan.
- Validate architecture consistency.

## Hard Boundary

The agent **proposes**; it never triggers repository implementation. Output goes
to a human review/approval step before the Coding Agent is involved
(ADR-004, [CLAUDE.md](../CLAUDE.md) Rule 6).

```
User Request → Architecture Agent → Proposed Architecture → Change Plan →
Human Review → (Reject | Modify | Approve) → [Approve] → Coding Agent
```

## Inputs

```
User request
+ Current Architecture IR
+ Project context (name, description, connected repo metadata if any)
+ Architecture constraints
```

## Outputs

```
Explanation (natural language, grounded in the current IR)
+ Proposed Architecture (a modified IR)
+ Structured Change Plan
+ Potential Risks
```

The agent must stay grounded in the actual architecture state. It must not invent
components or connections inconsistent with the current IR.

## Example

User:

> Add Redis caching for the product API.

Agent (conceptual):

```
Architecture Change Plan

ADD:
- Redis (cache)

MODIFY:
- Product API

CONNECTIONS:
- Product API → Redis
- Redis → Product data source

APPLICATION CHANGES:
- Redis client
- Cache read
- Cache invalidation

ENVIRONMENT:
- REDIS_URL

TESTING:
- Cache hit
- Cache miss
- Cache invalidation
```

## Agent Tools (conceptual — not implemented)

```
get_architecture()
get_component(id)
search_components(query)
create_component(spec)
update_component(id, changes)
delete_component(id)
create_connection(spec)
delete_connection(id)
validate_architecture()
generate_change_plan()
```

These operate on a working copy of the IR within a single agent run. Nothing is
persisted as the project's architecture until the change is approved.

## Change Plan Operations

See [../PROJECT_MEMORY.md](../PROJECT_MEMORY.md) §12 and the change types:
`ADD_COMPONENT`, `REMOVE_COMPONENT`, `UPDATE_COMPONENT`, `ADD_CONNECTION`,
`REMOVE_CONNECTION`, `UPDATE_CONNECTION`, `UPDATE_TECHNOLOGY`.
