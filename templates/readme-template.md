# [PROJECT NAME]

[Short project description and primary goal.]

## Overview

- **Purpose**: [What this project solves]
- **Primary users/consumers**: [Who uses this project]
- **Status**: [Active/experimental/internal/etc.]

## Repository Structure

```text
[Top-level folders and what they contain]
```

## Architecture / Component Boundaries

- **[Component/Module A]**: [responsibility]
- **[Component/Module B]**: [responsibility]
- **[Integration boundary]**: [external systems or APIs]
- **[Runtime/deployment boundary]**: [where/how it runs]

## Agent Workflow

Feature delivery follows a Speckit-first, multi-agent loop:

1. Engineer generates required artifacts with:
   - `speckit.specify`
   - `speckit.plan`
   - `speckit.tasks`
   Recommended quality pre-steps:
   - `speckit.clarify`
   - `speckit.analyze`
2. Orchestrator runs `speckit.orchestrate` to dispatch lane-specific work:
   - `speckit.implement.db`
   - `speckit.implement.infra`
   - `speckit.implement.backend`
   - `speckit.implement.frontend`
3. Reviewer runs `speckit.review` and adds remediation tasks if gaps are found.
4. Steps 2-3 repeat until the highest-numbered `reviews/iteration-<nnn>.md` has `Status: PASS` and no remediation tasks remain open.
5. Documentation agent runs `speckit.docs` to update:
   - `docs/`
   - `README.md`
   - `ARCHITECTURE.md`

Workflow details: `docs/agent-workflow.md` and `.codex/workflows/speckit-multi-agent.yaml`.

## Requirements

- **Language/runtime**: [version requirements]
- **Package/build tool**: [tool and version]
- **System dependencies**: [if any]
- **External services**: [if any]

## Setup

```bash
[setup commands]
```

## Run / Start (Optional)

```bash
[start or dev command]
```

[What this command does and key caveats.]

## Stop (Optional)

```bash
[stop command]
```

## Build (Optional)

```bash
[build command]
```

## Database / Migrations (Optional)

```bash
[migration command]
```

## Run Tests

```bash
[test command(s)]
```

[Any prerequisite notes for integration/e2e/performance tests.]

## Lint / Format / Static Checks (Optional)

```bash
[lint and/or format commands]
```

## Quality Gates

```bash
[all required CI-quality commands]
```

[Quality expectations and non-compliance notes.]

## API Contract (Optional)

- **Source of truth**: `[path to OpenAPI/GraphQL/proto/etc.]`
- **Generated artifacts**: `[generation command and outputs]`

## Verification / Smoke Test (Optional)

```bash
[key command(s) to verify the primary flow]
```

## Additional Documentation

- `docs/`: [what lives there]
- `ARCHITECTURE.md`: [architecture details]
