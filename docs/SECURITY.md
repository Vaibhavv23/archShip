# SECURITY.md

**Conceptual specification only. Not implemented.** This document states the
security model the product must follow as features are built.

## Guiding Principle — Authorization Chain

```
User
 ↓
Authorization
 ↓
Project
 ↓
Architecture
 ↓
Approved Change
 ↓
Agent Permissions
 ↓
Repository
```

Every action an agent takes must be traceable back through this chain to an
authenticated user, an authorized project, and (for implementation) an explicit
approval. The Coding Agent must only access repositories the user has explicitly
authorized for that project.

## Authentication

- User accounts with a vetted auth strategy (provider TBD — see
  [../PROJECT_MEMORY.md](../PROJECT_MEMORY.md) §30).
- Sessions/tokens signed with a server-side secret (`APP_SECRET`).

## Authorization

- Every API request is checked against project membership
  (`project_members`) and role.
- Agent runs inherit the initiating user's authorization; they cannot exceed it.

## Project Isolation

- All data (architectures, change plans, repository connections, agent runs) is
  scoped to a project. No cross-project reads.

## Repository Access

- Per-project repository connections, explicitly authorized and revocable.
- Least-privilege credentials/scopes (see [GITHUB.md](GITHUB.md)).

## GitHub Credentials

- Stored server-side only; never sent to the browser.
- Stored via a secret manager or encrypted at rest; not plain columns.
- Prefer short-lived tokens.

## Secret Handling

- No secrets in source code, logs, error messages, or commits.
- `.env` is git-ignored; only `.env.example` with placeholders is committed.
- LLM API keys and GitHub credentials live only in server configuration / secret
  storage.

## Agent Permissions

- Architecture Agent: read project + IR, write only to a working copy of the IR;
  cannot touch repositories or trigger implementation.
- Coding Agent: acts only on an approved change plan, only within the authorized
  repository, only via its bounded toolset.

## Tool Permissions

- Each agent tool has a narrow, documented capability.
- File writes limited to the working repository checkout.
- Git operations limited to feature branches; no force-push to protected
  branches; no merges (ADR-007).

## Command Execution

- The Coding Agent runs repository commands (tests, build, linters). These are
  arbitrary and untrusted.

## Sandboxing

- Repository command execution must run in a **secure, isolated execution
  environment** (container / microVM / hosted sandbox — choice is an open
  question). No access to the host, to other projects' data, or to platform
  secrets beyond what the run needs. Network egress restricted where possible.

## Audit Logs

- Every agent run and every agent action (tool call, file change, command)
  recorded (`agent_runs`, `agent_actions`).
- Approvals recorded with user, timestamp, and the exact plan approved.

## Data Retention

- Define retention for agent run logs, validation output, and repository
  checkouts. Repository working copies should be ephemeral and destroyed after a
  run. (Policy TBD.)

## LLM Data Boundaries

- Be explicit about what is sent to external LLM providers (IR, change plans,
  repository file contents).
- Never send platform secrets or other projects' data.
- Document provider data-use / retention terms before enabling a provider.
- Allow project-level opt-out where feasible.
