# DATA_MODEL.md

**Conceptual specification only. No database tables, migrations, or ORM models
have been implemented.** Primary database is PostgreSQL (ADR-002).

## Core Entities (planned)

| Entity | Purpose |
| --- | --- |
| `users` | Account identity. |
| `projects` | A workspace containing one architecture and its integrations. |
| `project_members` | User ↔ project membership and role. |
| `architectures` | The current architecture for a project. |
| `architecture_versions` | Immutable snapshots / version history of an architecture (see IR versioning). |
| `components` | Components within an architecture version. |
| `connections` | Connections between components within an architecture version. |
| `architecture_changes` | Individual structured change operations (ADD_COMPONENT, etc.). |
| `change_plans` | A proposed, reviewable set of changes produced by the Architecture Agent. |
| `approvals` | Explicit human decision on a change plan (approve / reject / modify). |
| `repositories` | Known GitHub repositories. |
| `repository_connections` | Project ↔ repository authorization and credentials reference. |
| `agent_runs` | A single execution of the Architecture Agent or Coding Agent. |
| `agent_actions` | Individual tool calls / steps within an agent run (audit trail). |
| `validation_runs` | Test / build / lint executions and their results. |
| `pull_requests` | PRs created by the Coding Agent, with metadata and status. |

## Relationships (high level)

```
users ──< project_members >── projects
projects 1──1 architectures 1──< architecture_versions
architecture_versions 1──< components
architecture_versions 1──< connections
projects 1──< change_plans 1──< architecture_changes
change_plans 1──1 approvals
projects 1──< repository_connections >── repositories
projects 1──< agent_runs 1──< agent_actions
agent_runs 1──< validation_runs
agent_runs 1──0..1 pull_requests
```

## Notes

- Secrets (GitHub tokens, LLM keys) are **not** stored as plain columns; store a
  reference to a secret manager or an encrypted value. See [SECURITY.md](SECURITY.md).
- `architecture_versions` + `architecture_changes` together support IR versioning
  ([ARCHITECTURE_IR.md](ARCHITECTURE_IR.md)); the exact snapshot vs. change-log
  balance is an open question.
- Every agent tool call should produce an `agent_actions` row for observability
  and audit.

## Potential Future Entities

```
technology_catalog       supported technologies and their metadata
architecture_templates   starting-point architectures
agent_memory             persistent agent context per project
architecture_rules       reusable constraint definitions
```

Do not implement any of this during initialization.
