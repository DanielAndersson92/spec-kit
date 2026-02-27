---

description: "Task list template for feature implementation"
---

# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Tests are MANDATORY. Every feature must include unit, integration,
contract, and end-to-end tests with requirement-ID traceability.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

**Traceability**: Every `FR-xxx`, `API-xxx`, and `SEC-xxx` requirement in
`spec.md` MUST have at least one mapped test task, and every test task MUST
reference valid requirement IDs. Implementation tasks MUST ensure each test
definition includes an explicit requirement reference tag/annotation. Security-
critical contract/e2e tasks MUST run through production authn/authz middleware
and real adapter boundaries. Required test suites MUST fail (not silently skip)
when prerequisites are missing unless an approved exception exists.

## Format: `[ID] [P?] [Lane] [Story?] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Lane]**: Exactly one lane tag per task: `[DB]`, `[INFRA]`, `[BE]`, or `[FE]`
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3); omit for setup/foundational/polish tasks
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/`, `tests/` at repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` or `android/src/`
- Paths shown below assume single project - adjust based on plan.md structure

<!-- 
  ============================================================================
  IMPORTANT: The tasks below are SAMPLE TASKS for illustration purposes only.
  
  The /speckit.tasks command MUST replace these with actual tasks based on:
  - User stories from spec.md (with their priorities P1, P2, P3...)
  - Feature requirements from plan.md
  - Entities from data-model.md
  - Endpoints from contracts/
  
  Tasks MUST be organized by user story so each story can be:
  - Implemented independently
  - Tested independently
  - Delivered as an MVP increment
  
  DO NOT keep these sample tasks in the generated tasks.md file.
  ============================================================================
-->

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 [INFRA] Create project structure per implementation plan
- [ ] T002 [INFRA] Initialize [language] project with [framework] dependencies
- [ ] T003 [P] [INFRA] Configure linting and formatting tools
- [ ] T004 [INFRA] Create/extend Makefile with deterministic CI/local targets (build, run, test-unit, test-integration, test-contract, test-e2e, lint, fmt, api-generate, migrate, up, down)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

Examples of foundational tasks (adjust based on your project):

- [ ] T005 [BE] Define/update OpenAPI contract in contracts/openapi.yaml before implementation
- [ ] T006 [P] [BE] Configure DTO/client generation from OpenAPI spec
- [ ] T007 [P] [BE] Setup API routing and middleware structure compliant with Zalando guidelines
- [ ] T008 [BE] Implement trusted identity extraction from verified tokens/assertions and reject caller-controlled identity headers as authority
- [ ] T009 [DB] Setup database schema and migrations framework through adapters
- [ ] T010 [BE] Implement deny-by-default authorization model `(actor, action, resource, tenant)`
- [ ] T011 [BE] Implement external callback/webhook authenticity checks (signature, freshness, replay protection) before state mutation
- [ ] T012 [BE] Configure error handling, validation, structured logging, correlation IDs, and tracing baseline
- [ ] T013 [BE] Implement auditable before/after state capture and explicit audit-write failure policy
- [ ] T014 [INFRA] Setup environment configuration and secrets handling (no secrets in VCS)
- [ ] T015 [INFRA] Create requirement-to-test traceability matrix for FR/API/SEC IDs
- [ ] TXXX [INFRA] Add CI check that every canonical OpenAPI operation is wired to a runtime route
- [ ] TXXX [INFRA] Add static/CI guard forbidding stub/placeholder production adapters outside test-only packages
- [ ] TXXX [DB] Add integration tests that prove transaction rollback/atomicity and tx-bound statement usage
- [ ] TXXX [INFRA] Add fail-closed test-runner behavior for missing integration/contract/e2e prerequisites
- [ ] TXXX [INFRA] Add CI dependency provisioning + health checks before integration/contract/e2e suites

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - [Title] (Priority: P1) 🎯 MVP

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Tests for User Story 1 (MANDATORY) ⚠️

> **NOTE: Write these tests FIRST, ensure they FAIL before implementation**

- [ ] T016 [P] [BE] [US1] Unit tests for domain/application logic with positive, negative, boundary cases in tests/unit/test_[name].py (refs: FR-xxx/API-xxx/SEC-xxx)
- [ ] T017 [P] [BE] [US1] Contract test validating OpenAPI schema + behavior/status/header/auth semantics through production middleware in tests/contract/test_[name].py (refs: API-xxx/SEC-xxx)
- [ ] T018 [P] [DB] [US1] Integration test for adapter behavior with real infrastructure in tests/integration/test_[name].py (refs: FR-xxx/SEC-xxx)
- [ ] T019 [P] [BE] [US1] End-to-end test over deployed boundary (real transport + adapters) for happy path and negative/authorization path in tests/e2e/test_[name].py (refs: FR-xxx/API-xxx/SEC-xxx)

### Implementation for User Story 1

- [ ] T020 [P] [BE] [US1] Create/extend domain model(s) in src/domain/[entity].py
- [ ] T021 [P] [BE] [US1] Implement use case(s) in src/application/[use_case].py
- [ ] T022 [P] [BE] [US1] Define/update port interfaces in src/ports/[port].py
- [ ] T023 [BE] [US1] Implement adapter(s) in src/adapters/[adapter].py
- [ ] T024 [BE] [US1] Implement transport/API endpoint in src/transport/[file].py
- [ ] T025 [BE] [US1] Regenerate DTO/client artifacts and verify no manual drift

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - [Title] (Priority: P2)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Tests for User Story 2 (MANDATORY) ⚠️

- [ ] T026 [P] [BE] [US2] Unit tests for domain/application logic in tests/unit/test_[name].py (refs: FR-xxx/API-xxx/SEC-xxx)
- [ ] T027 [P] [BE] [US2] Contract test validating OpenAPI schema + behavior/status/header/auth semantics through production middleware in tests/contract/test_[name].py (refs: API-xxx/SEC-xxx)
- [ ] T028 [P] [DB] [US2] Integration test with real adapters in tests/integration/test_[name].py (refs: FR-xxx/SEC-xxx)
- [ ] T029 [P] [BE] [US2] End-to-end test over deployed boundary (real transport + adapters) for happy + negative/authorization path in tests/e2e/test_[name].py (refs: FR-xxx/API-xxx/SEC-xxx)

### Implementation for User Story 2

- [ ] T030 [P] [BE] [US2] Extend domain model(s) in src/domain/[entity].py
- [ ] T031 [BE] [US2] Implement use case(s) in src/application/[use_case].py
- [ ] T032 [BE] [US2] Implement/update adapters in src/adapters/[adapter].py
- [ ] T033 [BE] [US2] Implement/update API behavior in src/transport/[file].py

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - [Title] (Priority: P3)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Tests for User Story 3 (MANDATORY) ⚠️

- [ ] T034 [P] [BE] [US3] Unit tests for domain/application logic in tests/unit/test_[name].py (refs: FR-xxx/API-xxx/SEC-xxx)
- [ ] T035 [P] [BE] [US3] Contract test validating OpenAPI schema + behavior/status/header/auth semantics through production middleware in tests/contract/test_[name].py (refs: API-xxx/SEC-xxx)
- [ ] T036 [P] [DB] [US3] Integration test with real adapters in tests/integration/test_[name].py (refs: FR-xxx/SEC-xxx)
- [ ] T037 [P] [BE] [US3] End-to-end test over deployed boundary (real transport + adapters) for happy + negative/authorization path in tests/e2e/test_[name].py (refs: FR-xxx/API-xxx/SEC-xxx)

### Implementation for User Story 3

- [ ] T038 [P] [BE] [US3] Extend domain model(s) in src/domain/[entity].py
- [ ] T039 [BE] [US3] Implement use case(s) in src/application/[use_case].py
- [ ] T040 [BE] [US3] Implement/update adapters in src/adapters/[adapter].py
- [ ] T041 [BE] [US3] Implement/update API behavior in src/transport/[file].py

**Checkpoint**: All user stories should now be independently functional

---

[Add more user story phases as needed, following the same pattern]

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] TXXX [P] [INFRA] Documentation updates in docs/ using .specify/templates/docs-template.md
- [ ] TXXX [BE] Code cleanup and refactoring
- [ ] TXXX [BE] Performance optimization across all stories
- [ ] TXXX [P] [BE] Verify domain branch coverage is >=80% in CI
- [ ] TXXX [P] [INFRA] Verify requirement-test traceability matrix completeness
- [ ] TXXX [P] [BE] Verify webhook/event authenticity tests cover signature, freshness, and replay protection
- [ ] TXXX [P] [BE] Verify audit trail includes actor/target/timestamp and before/after with explicit failure policy
- [ ] TXXX [P] [BE] Verify observability baseline (structured logs, correlation IDs, metrics, traces) for changed flows
- [ ] TXXX [P] [INFRA] README.md update using .specify/templates/readme-template.md for setup/runtime/config/API changes in same changeset
- [ ] TXXX [P] [INFRA] ARCHITECTURE.md update using .specify/templates/architecture-template.md
- [ ] TXXX [BE] Security hardening and deny-by-default authorization regression checks
- [ ] TXXX [P] [INFRA] Verify latest-stable dependency checks fail closed when registries are reachable (or document approved exception)
- [ ] TXXX [INFRA] Run quickstart.md validation

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - May integrate with US1 but should be independently testable
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - May integrate with US1/US2 but should be independently testable

### Within Each User Story

- Tests MUST be written and FAIL before implementation
- Unit + contract + integration + e2e tests are mandatory for each story
- Domain before application
- Models before services
- Services before endpoints
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Launch all mandatory tests for User Story 1 together:
Task: "Unit tests for [use case] in tests/unit/test_[name].py (refs: FR/API/SEC IDs)"
Task: "Contract test for [endpoint] in tests/contract/test_[name].py (refs: API IDs)"
Task: "Integration test for [adapter flow] in tests/integration/test_[name].py (refs: FR/SEC IDs)"
Task: "E2E test for [user journey] in tests/e2e/test_[name].py (refs: FR/API/SEC IDs)"

# Launch domain modeling tasks for User Story 1 together:
Task: "Create [Entity1] in src/domain/[entity1].py"
Task: "Create [Entity2] in src/domain/[entity2].py"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Lane] tag maps task to responsible implementation lane
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing and include requirement IDs
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence
