---
description: Orchestrate a multi-agent implementation, review, and documentation loop from tasks.md until no gaps remain, including DB/INFRA/BE/FE lanes.
handoffs:
  - label: Implement DB Lane
    agent: speckit.implement.db
    prompt: Implement the next unblocked DB tasks
    send: true
  - label: Implement Infra Lane
    agent: speckit.implement.infra
    prompt: Implement the next unblocked infra tasks
    send: true
  - label: Implement Backend Lane
    agent: speckit.implement.backend
    prompt: Implement the next unblocked backend tasks
    send: true
  - label: Implement Frontend Lane
    agent: speckit.implement.frontend
    prompt: Implement the next unblocked frontend tasks
    send: true
  - label: Run Review Loop
    agent: speckit.review
    prompt: Verify spec compliance and append remediation tasks for gaps
    send: true
  - label: Update Long-Lived Docs
    agent: speckit.docs
    prompt: Update docs/, README.md, and ARCHITECTURE.md after implementation is complete
    send: true
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

2. Load and analyze context from FEATURE_DIR:
   - **REQUIRED**: `spec.md`, `plan.md`, `tasks.md`
   - **IF EXISTS**: `data-model.md`, `contracts/`, `research.md`, `quickstart.md`
   - **IF EXISTS**: prior review reports in `reviews/`

3. Validate workflow entry criteria:
   - `tasks.md` exists and has at least one unchecked task.
   - Required checklists are complete, or user explicitly approves continuation.
   - Task IDs and dependency order are valid.

4. Build task routing lanes:
   - **DB lane**: tasks labeled `[DB]` or touching `migrations/`, `atlas.hcl`, `internal/adapters/postgres/`, DB-focused integration tests.
   - **Infra lane**: tasks labeled `[INFRA]` or touching `deploy/`, `.github/workflows/`, `build/`, `scripts/`, `Makefile`, runtime/config plumbing.
   - **Backend lane**: tasks labeled `[BE]` or touching `cmd/`, `internal/application/`, `internal/ports/`, `internal/adapters/http/`, `internal/bootstrap/`, contract/e2e API tests.
   - **Frontend lane**: tasks labeled `[FE]` or touching `frontend/`, `web/`, `ui/`, client SDK/UI tests.
   - **Fallback**: unresolved tasks are assigned by dominant file path and dependency constraints.

5. Run iterative implementation and review loop:
   - Dispatch only **unblocked** open tasks to implementation lanes.
   - Require each implementation lane to follow speckit-style rules: tests before code, dependency ordering, and task checkbox updates in `tasks.md`.
   - After implementation cycle, run `speckit.review`.
   - If review finds gaps, append remediation tasks to `tasks.md` and continue loop.
   - Repeat until review reports no gaps.

6. Exit implementation/review loop only when:
   - No unchecked implementation or review-remediation tasks remain.
   - Highest-numbered `reviews/iteration-<nnn>.md` has `Status: PASS` with no open findings.

7. Run `speckit.docs` to update long-lived documentation:
   - Update `docs/` content for behavior/operations changes.
   - Update `README.md`.
   - Update/create `ARCHITECTURE.md`.

8. Report final status:
   - Iteration count
   - Completed task count
   - Review finding trend (open vs resolved)
   - Documentation files changed

## Coordination Rules

- Never mark a task complete unless associated implementation and required tests are done.
- Keep review-added tasks in a dedicated section per iteration; do not overwrite historical tasks.
- Preserve user-authored task text unless required to resolve ambiguity.
- If lane ownership conflicts on the same file, enforce sequential execution for those tasks.
