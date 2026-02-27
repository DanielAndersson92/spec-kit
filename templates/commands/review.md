---
description: Review implementation against spec/plan/tasks, identify gaps or corner-cutting, and append remediation tasks until no gaps remain.
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

2. Load review context:
   - **REQUIRED**: `spec.md`, `plan.md`, `tasks.md`
   - **IF EXISTS**: `data-model.md`, `contracts/`, `research.md`, `quickstart.md`
   - **IF EXISTS**: prior reports in `reviews/`

3. Verify task and requirement coverage:
   - Check completed tasks for missing or weak validation.
   - Cross-check spec requirement IDs against tests and implementation behavior.
   - Flag deviations from plan architecture, security gates, or contract semantics.

4. Review for implementation quality gaps:
   - Missing negative-path handling
   - Missing authz/tenant boundary checks
   - Contract drift
   - Incomplete observability/audit behavior
   - Untested edge cases or brittle assumptions

5. Create review output at `FEATURE_DIR/reviews/iteration-<nnn>.md`:
   - Ensure `FEATURE_DIR/reviews/` exists.
   - Determine `<nnn>` by scanning existing `iteration-<nnn>.md` files, taking max, and incrementing by 1.
   - Use 3-digit zero-padded numbering (`001`, `002`, ...).
   - Include headers:
     - `Iteration: <nnn>`
     - `Status: FAIL` (set to `PASS` only when no findings remain)
   - Include:
     - Summary
     - Findings by severity
     - Requirement IDs impacted
     - Recommended remediation

6. If findings exist:
   - Append a new section to `tasks.md` named `Review Remediation (Iteration <nnn>)`.
   - Add actionable remediation tasks using the standard task format:
     `- [ ] T### [P?] [Lane] [Story?] Description with file path and requirement IDs`
   - Determine `T###` by scanning `tasks.md` for existing task IDs and continuing from the highest ID + 1.
   - Never reuse or renumber existing task IDs.
   - Use lane labels where possible: `[DB]`, `[BE]`, `[FE]`, `[INFRA]`.
   - Do not modify historical completed tasks.

7. If no findings exist:
   - Set `Status: PASS` in the current iteration report.
   - Confirm the implementation is ready for documentation updates.

## Guardrails

- Findings must be evidence-based and tied to spec/plan/task artifacts.
- Do not add vague remediation tasks; each must be executable by an implementation agent.
- Treat unresolved high-severity findings as blocking for completion.
- "Latest review report" always means the highest `<nnn>` in `iteration-<nnn>.md`.
