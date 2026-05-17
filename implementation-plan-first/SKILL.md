---
name: implementation-plan-first
description: Code-implementation planning workflow for Java Spring Boot and Angular, optimized for Windsurf Cascade. Use when the user wants a Markdown implementation plan file before coding, approval before implementation, interactive ask_question decisions, or plan-first coding with TDD guardrails.
metadata:
  short-description: File-based implementation plans before coding
---

# Implementation Plan First

Use this skill for coding tasks where the developer wants a code-implementation plan written to a Markdown file before any code changes. The plan should emphasize implementation shape, not detailed test-case design. TDD remains mandatory during implementation.

## Non-Negotiables

- Write the plan to `docs/plans/YYYY-MM-DD-<feature-slug>.md`; do not present the full plan in chat.
- No code changes before the developer approves the plan file.
- During implementation, no production code before a failing unit test for the current behavior.
- Database tools are for read-only planning discovery only, never for tests or implementation verification unless the developer changes scope.
- No integration tests, E2E tests, Testcontainers, real databases, real queues, real HTTP calls, browser automation, or full application context tests.
- If the user asks for integration/E2E coverage, say that is outside this skill and ask whether to switch scope.

## Workflow

### 1. Inspect Before Asking

Explore the repo first. Identify the framework, versions, build tool, likely target modules, public interfaces, existing test runner, and local implementation conventions. Do not ask questions answered by code, configs, docs, or tests.

If database lookup tools are available and schema or sample data would materially improve the implementation plan, ask permission once with `ask_question` before connecting. If approved, use only read-only lookup for schema metadata, constraints, indexes, safe counts, and tiny representative samples. If denied or unavailable, proceed with explicit assumptions. Read `references/database-lookup.md` for query-safety rules when needed.

### 2. Use Interactive Questions

When a Windsurf/Cascade question tool such as `ask_question` is available, use it for every developer-facing decision question, including grilling, plan approval, scope changes, and conflict resolution.

- Ask one question per tool call.
- Provide 2-4 mutually exclusive options.
- Put the recommended option first and label it with `(Recommended)`.
- Give each option a short impact/tradeoff description.
- Do not include filler choices. Use free-form fallback only when the decision cannot be represented by real options.
- If `ask_question` is unavailable, ask the same single question in chat with the recommendation and options clearly listed.

### 3. Grill The Developer

Before writing the plan file, resolve decisions that materially change the implementation:

- Ask one question at a time, using `ask_question` whenever possible.
- Provide the recommended answer with each question.
- Prefer questions about target modules, API/UI contract, data flow, validation behavior, compatibility constraints, rollout risk, and what must remain out of scope.
- Stop when the goal, code shape, public contracts, implementation boundaries, and verification checks are clear.

### 4. Write The Plan File

Create `docs/plans/` if needed. Write the plan to `docs/plans/YYYY-MM-DD-<feature-slug>.md`, using today's local date and a short kebab-case slug. Keep it implementation-focused and crisp:

```markdown
# <Feature Name> Implementation Plan

## Goal
...

## Current Context
...

## Current Data Or Schema Notes
...

## Proposed Code Changes
...

## Public Interfaces Or Contracts
...

## Implementation Steps
...

## TDD Guardrails
- Add one focused failing unit test before each behavior change.
- Implement the smallest code change that makes the current unit test pass.
- Refactor only while tests are green.

## Verification Commands
...

## Assumptions
...
```

The plan should describe code changes by module and behavior. Do not list detailed test cases. If a template would help, read `references/plan-template.md`.

### 5. Stop For Approval

After writing the plan file, stop. In chat, reply only with:

- the plan file path
- a 1-2 sentence summary
- an `ask_question` approval prompt when available

For approval, prefer `ask_question` with options like:

1. `Approve and implement (Recommended)` - start implementation from the plan using TDD guardrails.
2. `Revise plan` - update the Markdown plan before coding.
3. `Stop here` - leave the plan file as the final artifact.

Do not edit code until the developer explicitly approves through the tool or with words such as "approved", "approve", "go ahead", "proceed", or "implement this plan". If the developer changes the scope, revise the plan file and ask for approval again.

### 6. Execute With TDD Guardrails

After approval, follow the plan and use TDD for each behavior change:

1. RED: write exactly one focused unit test for observable behavior.
2. Verify RED: run the narrowest unit-test command and confirm the failure is for the missing behavior, not test setup.
3. GREEN: write the smallest production change that passes that unit test.
4. Verify GREEN: rerun the narrow test.
5. Refactor only while tests are green.
6. Repeat for the next behavior.

When unit test shape is unclear, read `references/unit-test-patterns.md`.

Run the broader existing unit-test command before completion. Never weaken, delete, or skip tests to make implementation pass unless the approved requirement changes.

Final response after implementation must include changed files, RED summary, GREEN summary, and verification commands run.

## Spring Boot Unit Rules

- Prefer isolated class tests with constructor-injected mocks.
- Use JUnit Jupiter, Mockito, AssertJ, and the repo's existing conventions.
- Mock immediate collaborators only when needed to isolate the unit.
- Do not use `@SpringBootTest`, Testcontainers, repository integration tests, real DBs, real queues, or real HTTP clients.
- For controllers, prefer direct method invocation or standalone MVC-style setup with mocked dependencies only when the repo already uses it for unit tests.

## Angular Unit Rules

- Use the existing runner: Vitest, Jasmine/Karma, or Jest.
- Test services and components in isolation; mock providers and HTTP dependencies.
- Use `TestBed` only as a unit-test harness, not as an app integration test.
- Prefer user-observable component behavior and outputs over private fields.
- Do not add Cypress, Playwright, backend calls, or browser automation.

## Database Lookup Rules

- Use database tools only before writing the plan file, and only after one task-level approval through `ask_question` when available.
- Allowed lookups: schemas, tables, columns, constraints, indexes, safe counts, and tiny sample rows needed to clarify implementation.
- Banned operations: writes, DDL, migrations, stored procedure execution, broad exports, secret/PII disclosure in plan output, and real database usage in tests.
- Record only concise implementation-relevant findings in `Current Data Or Schema Notes`.
- If database tools are denied, unavailable, or unsafe for the task, continue with explicit assumptions instead of blocking.
