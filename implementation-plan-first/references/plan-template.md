# Implementation Plan Template

Use this template only when writing a plan file. Keep the plan focused on code implementation; include TDD only as guardrails, not as a detailed test-case inventory.

```markdown
# <Feature Name> Implementation Plan

## Goal
State the requested outcome in one or two sentences.

## Current Context
Summarize the relevant modules, existing patterns, and constraints discovered from the repo.

## Current Data Or Schema Notes
Summarize only implementation-relevant database findings, such as table names, key columns, constraints, indexes, counts, or representative non-sensitive sample shapes. Use `None` if no database lookup was needed or available.

## Proposed Code Changes
- `<module or file area>`: describe the production code behavior to add or change.
- `<module or file area>`: describe any supporting refactor needed for the implementation.

## Public Interfaces Or Contracts
List API endpoints, component inputs/outputs, service methods, DTOs, request/response shapes, or user-visible behavior that will change.

## Implementation Steps
1. Update the core domain/service/component behavior.
2. Wire the behavior through the controller, component, service, or caller.
3. Add validation/error handling required by the public contract.
4. Refactor names or boundaries only where needed to keep the implementation clear.

## TDD Guardrails
- For each behavior change, write one focused failing unit test first.
- Implement only enough production code to pass the current failing unit test.
- Refactor only after the relevant unit tests are green.

## Verification Commands
- `<narrow unit test command>`
- `<broader unit test/build command>`

## Assumptions
- `<assumption or "None">`
```
