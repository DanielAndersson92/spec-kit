---
description: Generate an actionable, dependency-ordered tasks.md for the feature based on available design artifacts.
handoffs: 
  - label: Analyze For Consistency
    agent: speckit.analyze
    prompt: Run a project analysis for consistency
    send: true
  - label: Run Multi-Agent Delivery Loop
    agent: speckit.orchestrate
    prompt: Start lane-based implementation, review, and documentation workflow
    send: true
  - label: Implement Project (Single Agent)
    agent: speckit.implement
    prompt: Start implementation in phases with a single implementation agent
    send: true
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. **Setup**: Run `{SCRIPT}` from repo root and parse FEATURE_DIR and AVAILABLE_DOCS list. All paths must be absolute. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. **Load design documents**: Read from FEATURE_DIR:
   - **Required**: plan.md (tech stack, libraries, structure), spec.md (user stories with priorities)
   - **Optional**: data-model.md (entities), contracts/ (API endpoints), research.md (decisions), quickstart.md (test scenarios)
   - Note: Not all projects have all documents. Generate tasks based on what's available.

3. **Execute task generation workflow**:
   - Load plan.md and extract tech stack, libraries, project structure
   - Load spec.md and extract user stories with their priorities (P1, P2, P3, etc.)
   - If data-model.md exists: Extract entities and map to user stories
   - If contracts/ exists: Map interface contracts to user stories
   - If research.md exists: Extract decisions for setup tasks
   - Generate tasks organized by user story with explicit lane tags (see Task Generation Rules below)
   - Generate dependency graph showing user story completion order
   - Create parallel execution examples per user story
   - Validate task completeness (each user story has all needed tasks, independently testable)

4. **Generate tasks.md**: Use `templates/tasks-template.md` as structure, fill with:
   - Correct feature name from plan.md
   - Phase 1: Setup tasks (project initialization)
   - Phase 2: Foundational tasks (blocking prerequisites for all user stories)
   - Phase 3+: One phase per user story (in priority order from spec.md)
   - Each phase includes: story goal, independent test criteria, mandatory tests, implementation tasks
   - Final Phase: Polish & cross-cutting concerns
   - All tasks must follow the strict checklist format (see Task Generation Rules below)
   - Clear file paths for each task
   - Dependencies section showing story completion order
   - Parallel execution examples per story
   - Implementation strategy section (MVP first, incremental delivery)

5. **Report**: Output path to generated tasks.md and summary:
   - Total task count
   - Task count per user story
   - Task count per lane (`[DB]`, `[BE]`, `[FE]`, `[INFRA]`)
   - Parallel opportunities identified
   - Independent test criteria for each story
   - Suggested MVP scope (typically just User Story 1)
   - Format validation: Confirm ALL tasks follow the checklist format (checkbox, ID, labels, file paths)

Context for task generation: {ARGS}

The tasks.md should be immediately executable - each task must be specific enough that an LLM can complete it without additional context.

## Task Generation Rules

**CRITICAL**: Tasks MUST be organized by user story to enable independent implementation and testing.

**Tests are MANDATORY**: Generate unit, integration, contract, and e2e test tasks
for every user story. Enforce TDD ordering and requirement-ID traceability.

### Checklist Format (REQUIRED)

Every task MUST strictly follow this format:

```text
- [ ] [TaskID] [P?] [Lane] [Story?] Description with file path
```

**Format Components**:

1. **Checkbox**: ALWAYS start with `- [ ]` (markdown checkbox)
2. **Task ID**: Sequential number (T001, T002, T003...) in execution order
3. **[P] marker**: Include ONLY if task is parallelizable (different files, no dependencies on incomplete tasks)
4. **[Lane] label**: REQUIRED for every task
   - Allowed values: `[DB]`, `[BE]`, `[FE]`, `[INFRA]`
   - Use one lane label per task
5. **[Story] label**: REQUIRED for user story phase tasks only
   - Format: [US1], [US2], [US3], etc. (maps to user stories from spec.md)
   - Setup phase: NO story label
   - Foundational phase: NO story label  
   - User Story phases: MUST have story label
   - Polish phase: NO story label
6. **Description**: Clear action with exact file path

**Examples**:

- ✅ CORRECT: `- [ ] T001 [INFRA] Create project structure per implementation plan in deploy/docker-compose.yml`
- ✅ CORRECT: `- [ ] T005 [P] [BE] Implement authentication middleware in internal/adapters/http/gin/middleware/authz.go`
- ✅ CORRECT: `- [ ] T012 [P] [DB] [US1] Add membership indexes in migrations/versions/20260218170000_init_auth_schema.sql`
- ✅ CORRECT: `- [ ] T014 [BE] [US1] Implement MembershipService in internal/application/membership/service.go`
- ❌ WRONG: `- [ ] Create User model` (missing ID and lane tag)
- ❌ WRONG: `T001 [US1] Create model` (missing checkbox)
- ❌ WRONG: `- [ ] [US1] Create User model` (missing Task ID and lane tag)
- ❌ WRONG: `- [ ] T001 [US1] Create model` (missing lane tag and file path)

### Lane Tagging Rules (REQUIRED)

- Assign exactly one lane tag to every task.
- Use `[DB]` for schema/migrations/repositories/persistence behavior and DB-centric tests.
- Use `[BE]` for application services, HTTP handlers/middleware, contracts, and API/e2e behavior.
- Use `[FE]` for UI/client SDK/front-end state/UX behavior.
- Use `[INFRA]` for CI/CD, deployment manifests, runtime configuration, environment setup, make/build scripts, and cross-cutting operational wiring.
- Use file-path hints to resolve ambiguity:
  - `[DB]`: `migrations/`, `atlas.hcl`, `internal/adapters/postgres/`
  - `[BE]`: `cmd/`, `internal/application/`, `internal/ports/`, `internal/adapters/http/`, `internal/bootstrap/`
  - `[FE]`: `frontend/`, `web/`, `ui/`
  - `[INFRA]`: `deploy/`, `.github/workflows/`, `build/`, `scripts/`, `Makefile`, `.env.example`
- Setup/foundational/polish tasks default to `[INFRA]` unless a more specific lane clearly applies.

### Task Organization

1. **From User Stories (spec.md)** - PRIMARY ORGANIZATION:
   - Each user story (P1, P2, P3...) gets its own phase
   - Map all related components to their story:
     - Models needed for that story
     - Services needed for that story
     - Endpoints/UI needed for that story
     - Mandatory tests for that story (unit, integration, contract, e2e)
   - Mark story dependencies (most stories should be independent)

2. **From Contracts**:
   - Map each contract/endpoint → to the user story it serves
   - Each contract MUST map to at least one contract compatibility test task [P]
   - Contract test tasks MUST reference relevant `API-xxx` requirement IDs

3. **From Data Model**:
   - Map each entity to the user story(ies) that need it
   - If entity serves multiple stories: Put in earliest story or Setup phase
   - Relationships → service layer tasks in appropriate story phase

4. **From Setup/Infrastructure**:
   - Shared infrastructure → Setup phase (Phase 1)
   - Foundational/blocking tasks → Foundational phase (Phase 2)
   - Story-specific setup → within that story's phase
   - Tag operational/setup tasks as `[INFRA]`

### Phase Structure

- **Phase 1**: Setup (project initialization)
- **Phase 2**: Foundational (blocking prerequisites - MUST complete before user stories)
- **Phase 3+**: User Stories in priority order (P1, P2, P3...)
  - Within each story: Tests (mandatory) → Domain/Application → Endpoints → Integration
  - Each phase should be a complete, independently testable increment
- **Final Phase**: Polish & Cross-Cutting Concerns
