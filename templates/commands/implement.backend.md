---
description: Implement unblocked backend/API/service tasks from tasks.md using the Speckit implementation rules.
scripts:
  sh: scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
  ps: scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. Run `{SCRIPT}` from repo root and parse FEATURE_DIR and AVAILABLE_DOCS list. All paths must be absolute. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. Read implementation context:
   - **REQUIRED**: `tasks.md`, `plan.md`, `spec.md`
   - **IF EXISTS**: `data-model.md`, `contracts/`, `research.md`, `quickstart.md`

3. Select only backend-lane tasks that are open and unblocked:
   - Explicit lane tags: `[BE]`
   - File path focus: `cmd/`, `internal/application/`, `internal/ports/`, `internal/adapters/http/`, `internal/bootstrap/`, API-focused tests

4. Execute selected tasks with Speckit implementation discipline:
   - Respect dependency order and phase boundaries.
   - Follow TDD sequencing where tests are required.
   - Keep behavior aligned with OpenAPI/contracts and spec requirements.
   - Coordinate with other lanes on shared files before editing.

5. Validate backend-lane outcomes:
   - Run unit/contract/e2e suites relevant to changed behavior.
   - Confirm authn/authz, tenant scoping, and error semantics remain compliant.

6. For each completed task:
   - Mark as `[X]` in `tasks.md`.
   - Keep task text unchanged except for checkbox state.
   - Report evidence (files changed + tests run).

7. If no backend-lane tasks are available:
   - Return a no-op status with the next blocking dependency.

## Guardrails

- Do not take frontend-only or DB-only tasks unless explicitly reassigned.
- Do not mark blocked tasks complete.
- Do not cut required security, contract, or observability behavior.
