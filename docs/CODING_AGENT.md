# CODING_AGENT.md

**Conceptual specification only. Not implemented.**

## Purpose

Turn an approved Architecture Change Plan into real repository changes and open a
GitHub Pull Request. The Coding Agent is an implementation layer, separate from
the Architecture Agent (ADR-006). It uses existing coding-capable LLM APIs
(ADR-005); we do not train or build a proprietary foundation model in V1.

## Inputs

```
Architecture IR
+ Approved Change Plan
+ Repository (user-authorized)
+ Project Configuration (languages, test command, build command, constraints)
```

## Workflow

```
Approved Change Plan
    → Repository Discovery
    → Repository Understanding
    → Implementation Plan
    → Modify Code
    → Run Tests
    → Run Build
    → Analyze Failures
    → Fix Implementation
    → Final Validation
    → Git Commit
    → GitHub Pull Request
```

Compact form: `Understand → Plan → Modify → Test → Fix → Validate → Commit → PR`.

The agent must inspect existing code before editing it. It must never blindly
regenerate an entire application when a focused change is required
([CLAUDE.md](../CLAUDE.md) Rule 7).

## Tool Categories (conceptual — not implemented)

```
Repository / Filesystem
  read_file()      search_code()    list_files()
  create_file()    modify_file()    delete_file()

Execution / Terminal
  run_command()    run_tests()      run_build()      run_linter()

Git
  create_branch()  git_diff()       git_commit()

GitHub
  create_pull_request()   update_pull_request()
```

## Safety Boundaries

The Coding Agent must not:

- modify unrelated files without a stated reason,
- expose or commit secrets,
- bypass the user approval step,
- deploy to production (out of scope for V1, ADR-009),
- merge its own Pull Request in V1 (ADR-007).

Additional guardrails:

- Bounded retry limits on the fix loop.
- All tool calls recorded as agent actions for audit/observability.
- Runs in a secure, isolated execution environment (see [SECURITY.md](SECURITY.md)).
- Only operates on repositories the user has explicitly authorized.

## Output

A branch, one or more commits, and a Pull Request whose description follows the
structure in [GITHUB.md](GITHUB.md): title, summary, architecture changes,
implementation changes, database changes, API changes, tests, validation results,
architecture impact.
