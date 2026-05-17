---
name: gherkin-e2e-feature
description: Generate behavior-focused Gherkin feature files and Java Cucumber step definitions for implemented features, using ask_question decisions, Forge AI feature upload/approval, and contract-tool lookup when available.
metadata:
  short-description: Generate Gherkin and Java Cucumber steps
---

# Gherkin E2E Feature

Use this skill after a feature is implemented and the developer wants a behavior-focused Gherkin `.feature` file plus Java Cucumber step definitions for end-to-end validation.

## Non-Negotiables

- Inspect the repo before asking questions or generating files.
- Prefer `ask_question` for developer-facing decisions, approvals, ambiguous tool choices, and missing-contract decisions.
- Generate feature files at `docs/features/<feature-slug>.feature`.
- Generate Java Cucumber step definition source artifacts at `docs/step-definitions/<FeatureSlug>StepDefinitions.java`.
- Use Forge AI upload/approval tools and contract lookup tools when available; never invent tool names, schemas, responses, or approvals.
- If a required Forge or contract tool is missing, ambiguous, or unsafe, stop and report the exact missing capability.
- Do not place executable step definitions under `src/test/java` unless the developer explicitly asks and the existing Cucumber layout has been inspected.

## Workflow

### 1. Inspect The Implemented Feature

Find the implemented behavior, entrypoints, API/UI contract, existing tests, build tool, existing Cucumber setup, step definition conventions, helper clients, and docs. Search for existing `.feature` files and step definitions before generating new step text.

If the feature boundary, API-vs-UI coverage, scenario level, or target behavior is unclear after inspection, ask one decision-changing question at a time with `ask_question` when available. Provide 2-4 real options and put the recommended option first.

### 2. Read Contract Data

Before writing step definitions, use the available contract lookup tool to discover canonical endpoints, request/response shapes, status codes, events, UI states, identifiers, and assertion fields.

If multiple contract tools or contracts match, ask the developer which one to use. If lookup is unavailable, ask whether to continue from repo-discovered contracts or stop until contract data is available.

### 3. Generate The Gherkin Feature

Write one `.feature` file under `docs/features/` using business-readable behavior:

- Describe what the system does, not how it is implemented.
- Keep each scenario focused on one observable behavior.
- Prefer declarative Given/When/Then wording over low-level clicks, fields, HTTP calls, method names, or database details.
- Use `Background` only for common context that is essential to every scenario.
- Use `Rule` when it clarifies a business rule with multiple scenarios.
- Use `Scenario Outline` only for equivalent data variations.

Read `references/gherkin-guidelines.md` when scenario shape or wording is unclear.

### 4. Upload And Approve Through Forge AI

After generating the feature file, invoke the Forge AI feature upload tool if available. Inspect the returned validation, generated ID, comments, or review status.

Approve the feature only through the tool-provided approval operation after the Forge response is valid for the requested feature. If Forge requires human approval, use `ask_question` and wait for explicit approval. If the tool is unavailable or ambiguous, stop and report what is missing.

Read `references/forge-contract-workflow.md` for the tool-gated workflow and failure handling.

### 5. Generate Step Definitions

After Forge upload/approval and contract lookup, generate Java Cucumber step definitions under `docs/step-definitions/`:

- Match the generated Gherkin steps exactly.
- Prefer Cucumber Expressions over regular expressions unless the project convention differs.
- Reuse existing step wording and helper boundaries; do not create duplicate step definitions.
- Keep step methods thin. Delegate HTTP, browser, data setup, and assertions to existing helpers, clients, page objects, or a small helper skeleton if needed.
- Use contract-derived request/response shapes and assertions.
- Keep secrets, tokens, PII, and environment-specific values out of generated files.

Read `references/java-step-definitions.md` when Java Cucumber shape is unclear.

### 6. Upload Step Definitions

If Forge or the contract workflow provides a step-definition upload operation, upload the generated step definition artifact after validating that it matches the approved feature and contract data. If upload is unavailable, leave the local artifact and report the missing upload capability.

### 7. Final Response

Keep the final response short. Include generated file paths, Forge upload/approval result if available, contract lookup source used, and any missing tools or assumptions. Do not paste full feature files or step definitions into chat unless the developer asks.
