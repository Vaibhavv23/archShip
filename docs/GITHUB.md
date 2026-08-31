# GITHUB.md

**Conceptual specification only. Not implemented. All concrete API details below
are marked and must be verified against current official GitHub documentation
during implementation ([CLAUDE.md](../CLAUDE.md) Rule 10).**

## Scope

V1 GitHub integration: connect a repository, let the Coding Agent read and modify
it in an isolated environment, create a branch and commit, and open a Pull
Request. V1 output is always a PR, never a direct push to the default branch and
never a self-merge (ADR-007).

## Repository Connection

- The user authorizes access to specific repositories from a project.
- The system stores a repository connection record (owner, repo, default branch,
  connection/installation identifier).
- Access is per-project and revocable.

> To be verified against current official GitHub documentation during
> implementation: whether to use a GitHub App installation, OAuth app, or
> fine-grained personal access tokens; exact scopes/permissions required for
> reading content, creating branches, and opening PRs.

## Authentication Strategy

- Server-side only. Tokens/credentials never reach the browser
  ([CLAUDE.md](../CLAUDE.md) Rule 9, [SECURITY.md](SECURITY.md)).
- Prefer short-lived, narrowly-scoped credentials.

> To be verified against current official GitHub documentation during
> implementation: token lifetimes, refresh behavior, installation token
> generation.

## Repository Discovery

- List repositories the user/installation can access.
- Fetch default branch and basic metadata for a selected repository.

> To be verified against current official GitHub documentation during
> implementation: endpoints, pagination, rate limits.

## Branch, Files, Commit

- Create a feature branch from the default branch (naming e.g.
  `arch/<change-plan-id>`).
- Apply file changes produced by the Coding Agent.
- Create one or more commits with descriptive messages.

> To be verified against current official GitHub documentation during
> implementation: git data API vs. contents API vs. pushing from a cloned repo
> in the execution environment; how to create commits with multiple file
> changes atomically.

## Pull Request Creation

- Open a PR from the feature branch into the default (or user-selected) base
  branch.
- Populate title and a structured description.

### PR Metadata / Description Structure

```
Title
Summary
Architecture Changes
Implementation Changes
Database Changes
API Changes
Tests
Validation Results
Architecture Impact
```

> To be verified against current official GitHub documentation during
> implementation: PR creation endpoint, draft PR support, labels/assignees.

## Validation Status

- Attach the Coding Agent's validation results (test/build output summary) to the
  PR description and/or as a comment.

> To be verified against current official GitHub documentation during
> implementation: whether to also surface status via commit statuses or checks.

## Future: Webhooks

- Later, subscribe to PR events (review, merge, close) to update project state
  and enable code-to-architecture sync.

> To be verified against current official GitHub documentation during
> implementation: webhook event types and payload shapes. Do not guess payloads.
