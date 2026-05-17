# Forge And Contract Workflow

Use this reference when Forge AI or contract tools are available in the target environment. The exact tool names and schemas are environment-specific; discover them at runtime instead of inventing calls.

## Contract Lookup

Use contract data before generating step definitions. Prefer canonical contract sources in this order when available:

1. Contract lookup tool for the implemented feature.
2. Repo-local OpenAPI, AsyncAPI, GraphQL, Pact, protobuf, or UI contract files.
3. Existing controller/component contracts and tests.
4. Developer confirmation through `ask_question`.

Capture only what step definitions need:

- Endpoint, route, or UI capability name.
- Request inputs and required fields.
- Response shape, event shape, or UI state.
- Status codes and error contracts.
- Identifiers used to correlate setup, action, and assertion.

## Forge Upload And Approval

1. Generate the `.feature` file locally.
2. Invoke the available Forge AI feature upload tool.
3. Read the tool response for feature ID, validation errors, review comments, and approval state.
4. If validation fails, update the feature file and upload again.
5. If the tool exposes an approval operation and the uploaded feature is valid, approve through that operation.
6. If human approval is required, ask through `ask_question` and wait.
7. Generate step definitions only after the feature is uploaded and approved, or after the developer explicitly chooses to proceed without Forge approval.
8. Upload step definitions if the environment exposes a step-definition upload operation.

## Stop Conditions

Stop instead of guessing when:

- No Forge upload tool is available and upload is required.
- More than one Forge tool could apply and the correct one is unclear.
- Contract lookup is unavailable and the developer has not approved continuing from repo-discovered contracts.
- Forge validation fails with unresolved comments.
- Approval requires a human decision.
- Step-definition upload support is missing but the developer required remote upload.

Final output should clearly name the missing capability and list the local artifact paths already generated.
