# Feature Specification: [FEATURE NAME]

**Feature Branch**: `[###-feature-name]`  
**Created**: [DATE]  
**Status**: Draft  
**Input**: User description: "$ARGUMENTS"

## User Scenarios & Testing *(mandatory)*

<!--
  IMPORTANT: User stories should be PRIORITIZED as user journeys ordered by importance.
  Each user story/journey must be INDEPENDENTLY TESTABLE - meaning if you implement just ONE of them,
  you should still have a viable MVP (Minimum Viable Product) that delivers value.
  
  Assign priorities (P1, P2, P3, etc.) to each story, where P1 is the most critical.
  Think of each story as a standalone slice of functionality that can be:
  - Developed independently
  - Tested independently
  - Deployed independently
  - Demonstrated to users independently
-->

### User Story 1 - [Brief Title] (Priority: P1)

[Describe this user journey in plain language]

**Why this priority**: [Explain the value and why it has this priority level]

**Independent Test**: [Describe how this can be tested independently - e.g., "Can be fully tested by [specific action] and delivers [specific value]"]

**Acceptance Scenarios**:

1. **Given** [initial state], **When** [action], **Then** [expected outcome]
2. **Given** [initial state], **When** [action], **Then** [expected outcome]

---

### User Story 2 - [Brief Title] (Priority: P2)

[Describe this user journey in plain language]

**Why this priority**: [Explain the value and why it has this priority level]

**Independent Test**: [Describe how this can be tested independently]

**Acceptance Scenarios**:

1. **Given** [initial state], **When** [action], **Then** [expected outcome]

---

### User Story 3 - [Brief Title] (Priority: P3)

[Describe this user journey in plain language]

**Why this priority**: [Explain the value and why it has this priority level]

**Independent Test**: [Describe how this can be tested independently]

**Acceptance Scenarios**:

1. **Given** [initial state], **When** [action], **Then** [expected outcome]

---

[Add more user stories as needed, each with an assigned priority]

### Edge Cases

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right edge cases.
-->

- What happens when [boundary condition]?
- How does system handle [error scenario]?

## Requirements *(mandatory)*

<!--
  ACTION REQUIRED: The content in these sections represents placeholders.
  Fill them out with the right requirements.
-->

### Functional Requirements (FR-xxx)

- **FR-001**: System MUST [specific capability, e.g., "allow users to create accounts"]
- **FR-002**: System MUST [specific capability, e.g., "validate email addresses"]  
- **FR-003**: Users MUST be able to [key interaction, e.g., "reset their password"]
- **FR-004**: System MUST [data requirement, e.g., "persist user preferences"]
- **FR-005**: System MUST [behavior, e.g., "log all security events"]

*Example of marking unclear requirements:*

- **FR-006**: System MUST authenticate users via [NEEDS CLARIFICATION: auth method not specified - email/password, SSO, OAuth?]
- **FR-007**: System MUST retain user data for [NEEDS CLARIFICATION: retention period not specified]

### API Requirements (API-xxx)

- **API-001**: OpenAPI specification MUST define [resource/operation] request/response schemas.
- **API-002**: API MUST comply with Zalando RESTful API Guidelines for [naming/errors/pagination].
- **API-003**: API changes MUST remain backward compatible within the current major version.
- **API-004**: DTOs and clients MUST be generated from the OpenAPI source of truth.
- **API-005**: Every canonical OpenAPI operation MUST be reachable through runtime route wiring.
- **API-006**: Contract verification MUST validate request/response schema, status codes,
  and required auth/header semantics where defined; path-string existence checks are non-compliant.

### Security Requirements (SEC-xxx)

- **SEC-001**: System MUST validate all boundary inputs and reject malformed payloads.
- **SEC-002**: System MUST enforce least-privilege authorization for all protected operations.
- **SEC-003**: System MUST mitigate applicable OWASP Top 10 risks for this feature.
- **SEC-004**: Secrets MUST NOT be stored in source control or test fixtures.
- **SEC-005**: Identity/actor context MUST come only from cryptographically verified tokens
  or trusted signed assertions, with issuer/audience/signature/expiry validation, and
  never directly from caller-controlled identity headers.
- **SEC-006**: Protected operations MUST enforce deny-by-default authorization as
  `(actor, action, resource, tenant)`; missing context MUST deny.
- **SEC-007**: Inbound provider callbacks/events MUST verify signature, freshness window,
  and replay protection before any state mutation.
- **SEC-008**: Security/permission/billing state changes MUST record actor, target,
  timestamp, and before/after state with explicit audit-failure handling behavior.
- **SEC-009**: Services MUST emit structured logs, correlation IDs, core
  security/authorization metrics, and trace signals for request flows.

### Traceability Requirements

- Every `FR-xxx`, `API-xxx`, and `SEC-xxx` requirement MUST map to at least one test.
- Every test case MUST include an explicit requirement reference tag/annotation
  and reference at least one valid requirement ID.
- Each requirement MUST include positive, negative, and boundary test scenarios.
- Contract tests MUST validate API behavior against the committed OpenAPI specification.
- Contract and e2e tests for security-critical paths MUST execute through production
  authn/authz middleware and real adapter boundaries.
- Required unit/integration/contract/e2e suites MUST fail when prerequisites are
  missing, unless an approved time-bounded exception exists.
- E2E tests MUST exercise deployed service boundaries over real transport and real
  adapters; in-process router tests with fake repositories are non-compliant.

## API Contract *(mandatory for backend/API features)*

- **Source of Truth**: [Path to OpenAPI document in `contracts/`]
- **Compatibility Notes**: [How backward compatibility is preserved in current major version]
- **Generated Artifacts**: [DTO/client generation command and output locations]

### Key Entities *(include if feature involves data)*

- **[Entity 1]**: [What it represents, key attributes without implementation]
- **[Entity 2]**: [What it represents, relationships to other entities]

## Success Criteria *(mandatory)*

<!--
  ACTION REQUIRED: Define measurable success criteria.
  These must be technology-agnostic and measurable.
-->

### Measurable Outcomes

- **SC-001**: [Measurable metric, e.g., "Users can complete account creation in under 2 minutes"]
- **SC-002**: [Measurable metric, e.g., "System handles 1000 concurrent users without degradation"]
- **SC-003**: [User satisfaction metric, e.g., "90% of users successfully complete primary task on first attempt"]
- **SC-004**: [Business metric, e.g., "Reduce support tickets related to [X] by 50%"]
