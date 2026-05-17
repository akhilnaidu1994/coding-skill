# Java Cucumber Step Definitions

Use this reference when generating Java step definition source artifacts under `docs/step-definitions/`. Match the target repo's existing Cucumber, assertion, dependency injection, package, and helper conventions when they exist.

## Preferred Shape

```java
package com.example.e2e.steps;

import static org.assertj.core.api.Assertions.assertThat;

import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;

public class OrderStepDefinitions {
  private final OrderApiClient orders = new OrderApiClient();
  private CustomerFixture customer;
  private CartFixture cart;
  private OrderResponse response;

  @Given("a customer is eligible to place orders")
  public void aCustomerIsEligibleToPlaceOrders() {
    customer = CustomerFixture.eligibleCustomer();
  }

  @Given("the customer has a cart with one available item")
  public void theCustomerHasACartWithOneAvailableItem() {
    cart = CartFixture.withAvailableItem(customer);
  }

  @When("the customer places the order")
  public void theCustomerPlacesTheOrder() {
    response = orders.placeOrder(customer, cart);
  }

  @Then("the order is accepted")
  public void theOrderIsAccepted() {
    assertThat(response.status()).isEqualTo(201);
    assertThat(response.orderId()).isNotBlank();
  }

  @Then("the customer receives an order confirmation")
  public void theCustomerReceivesAnOrderConfirmation() {
    assertThat(response.confirmationId()).isNotBlank();
  }
}
```

## Rules

- Prefer Cucumber Expressions, such as `{int}`, `{word}`, and `{string}`, over regular expressions.
- Use custom parameter types only when the domain value appears repeatedly and improves readability.
- Keep step methods thin and readable; delegate transport, authentication, setup, polling, cleanup, and parsing to helpers.
- Reuse existing clients, fixtures, page objects, and assertion helpers when present.
- Store per-scenario state in instance fields only when that matches the repo's Cucumber object lifecycle. Otherwise reuse the existing scenario context pattern.
- Make assertions contract-based and user-observable.
- Use `DataTable` for readable structured inputs and DocString for payload-like content only when the scenario remains readable.
- Do not duplicate existing step definitions. Search first and reuse exact step text where possible.
- Do not hard-code secrets, real credentials, real customer data, or environment-specific IDs.

## Duplicate-Step Check

Before adding a new step, search existing step definitions for:

- Same annotation text with different Java method names.
- Same business meaning with slightly different wording.
- Regex steps that already match the proposed wording.
- Common reusable setup or assertion steps.

If a near-duplicate exists, adapt the generated Gherkin to reuse the existing step wording unless that would make the scenario less accurate.
