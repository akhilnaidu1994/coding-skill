# Gherkin Guidelines

Use this reference when creating or reviewing `.feature` files. Base wording on the implemented feature and contract, not on internal implementation details.

## Source Guidance

- Cucumber's "Writing better Gherkin" guidance says scenarios should describe intended behavior rather than implementation, and should favor declarative wording that reads as living documentation.
- Cucumber step definitions map Gherkin step text to automation code through expressions, so stable business wording helps keep automation maintainable.

References:
- https://cucumber.io/docs/bdd/better-gherkin/
- https://cucumber.io/docs/cucumber/step-definitions/

## Feature Shape

```gherkin
Feature: Customer can place an order
  Customers can submit valid orders and receive confirmation.

  Rule: Orders require a valid customer and at least one item

    Scenario: Customer places a valid order
      Given a customer is eligible to place orders
      And the customer has a cart with one available item
      When the customer places the order
      Then the order is accepted
      And the customer receives an order confirmation
```

## Rules

- Use a `Feature` title that names the business capability.
- Add a short feature description only when it clarifies business value or scope.
- Write scenarios around observable outcomes, not classes, methods, SQL, DTOs, or framework behavior.
- Prefer domain wording: "the order is accepted" instead of "the API returns 201" unless status code is the contract being tested.
- Keep each scenario focused on one behavior and one primary outcome.
- Keep setup minimal. Move repeated setup into `Background` only if every scenario needs it.
- Use `Rule` for named business rules with multiple scenarios.
- Use `Scenario Outline` only for equivalent examples that differ by input/output data.
- Use tables for readable domain data, not for dumping full payloads.
- Do not include secrets, real customer data, tokens, hostnames, or environment-specific IDs.

## Anti-Patterns

- Low-level UI scripts: `When I click the Submit button` when the business action is `When the customer places the order`.
- Implementation leaks: controller names, service methods, database table names, queue names, request builders, internal flags.
- Multiple unrelated outcomes in one scenario.
- Scenario outlines used as broad test matrices.
- Step text that differs only by tiny wording changes from existing reusable steps.
