# Documentation Update: [FEATURE NAME]

**Feature Branch**: `[###-feature-name]`  
**Date**: [DATE]  
**Primary Spec**: `specs/[###-feature-name]/spec.md`  
**Implementation Plan**: `specs/[###-feature-name]/plan.md`  
**Task Source**: `specs/[###-feature-name]/tasks.md`

## Purpose

[Describe what changed and why long-lived documentation must be updated.]

## Scope of Documentation Changes

### Docs Directory Updates (`docs/`)

- **Updated files**:
  - `[docs/file-1.md]`: [what changed]
  - `[docs/file-2.md]`: [what changed]
- **New files**:
  - `[docs/new-file.md]`: [purpose]
- **No longer accurate files reviewed**:
  - `[docs/file.md]`: [decision: updated/kept/deprecated]

### Repository Entry-Point Updates

- **`README.md`**:
  - [setup/runtime/API/workflow changes reflected]
- **`ARCHITECTURE.md`**:
  - [runtime and delivery architecture changes reflected]

## Source-of-Truth Mapping

Map every documentation change to implementation evidence:

- **Feature behavior**: [code paths, tests, contracts]
- **Operational behavior**: [deploy/CI/scripts/Makefile changes]
- **Security/compliance behavior**: [authn/authz, webhook, audit, observability updates]

## Command and Path Validation

- [ ] Every documented command exists and is valid for this repository.
- [ ] Every documented file path exists.
- [ ] Cross-links between docs resolve correctly.
- [ ] No section describes planned behavior that is not implemented.

## Change Summary for Release Notes

- [Concise bullets of externally visible behavior changes]

## Follow-ups

- [Optional documentation debt or deferred clarifications]
