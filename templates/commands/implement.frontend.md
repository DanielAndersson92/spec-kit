---
description: Implement unblocked frontend/client tasks from tasks.md using the Speckit implementation rules.
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

3. Select only frontend-lane tasks that are open and unblocked:
   - Explicit lane tags: `[FE]`
   - File path focus: `frontend/`, `web/`, `ui/`, client SDK/UI tests

4. Execute selected tasks with Speckit implementation discipline:
   - Respect dependency order and phase boundaries.
   - Follow TDD sequencing where tests are required.
   - Keep UX/state flows aligned with spec scenarios and API contracts.
   - Coordinate with other lanes on shared files before editing.

5. Validate frontend-lane outcomes:
   - Run relevant frontend/unit/e2e suites for changed UI/client behavior.
   - Confirm accessibility, validation, and error handling paths are covered.

6. For each completed task:
   - Mark as `[X]` in `tasks.md`.
   - Keep task text unchanged except for checkbox state.
   - Report evidence (files changed + tests run).

7. If no frontend-lane tasks are available:
   - Return a no-op status with the next blocking dependency.

## Guardrails

- Do not take backend-only or DB-only tasks unless explicitly reassigned.
- Do not mark blocked tasks complete.
- Do not bypass UX acceptance scenarios or required validation flows.
