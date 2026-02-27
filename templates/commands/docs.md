---
description: Update long-lived documentation after implementation/review completion, including docs/, README.md, and ARCHITECTURE.md.
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

2. Validate documentation preconditions:
   - `tasks.md` has no open implementation/review-remediation tasks.
   - Highest-numbered `reviews/iteration-<nnn>.md` has `Status: PASS` (or user explicitly approves documenting known gaps).

3. Load source artifacts for documentation:
   - **REQUIRED**: `spec.md`, `plan.md`, `tasks.md`
   - **IF EXISTS**: `data-model.md`, `contracts/`, `research.md`, `quickstart.md`, `reviews/`
   - **REQUIRED TEMPLATES**:
     - `templates/docs-template.md`
     - `templates/architecture-template.md`
     - `templates/readme-template.md`
   - Inspect actual implemented code paths to ensure docs reflect reality.

4. Apply documentation updates:
   - Create/update `FEATURE_DIR/documentation.md` using `templates/docs-template.md`.
   - Use that feature documentation artifact to drive updates in `docs/`.
   - Use `templates/architecture-template.md` to create/update `ARCHITECTURE.md` at repo root.
   - Use `templates/readme-template.md` to create/update `README.md` with implemented setup, behavior, and workflow expectations.

5. Documentation quality checks:
   - Commands and paths are valid for this repo.
   - Behavior statements match implemented behavior (not planned-only behavior).
   - Cross-file links are correct.

6. Report:
   - Files changed
   - Key documentation deltas
   - Any known follow-ups that could not be documented confidently

## Guardrails

- Do not invent behavior not present in code/spec artifacts.
- Keep architecture docs stable and long-lived; avoid iteration-specific noise.
- Treat README and ARCHITECTURE updates as required deliverables, not optional.
