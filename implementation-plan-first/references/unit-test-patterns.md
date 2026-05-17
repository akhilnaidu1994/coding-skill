# Unit Test Patterns

Load this file only during implementation when the unit test shape is unclear. Keep generated plan files implementation-focused; do not copy these examples into the plan unless a tiny snippet clarifies a public contract.

## Spring Service

Use plain JUnit 5, Mockito, AssertJ, and manual construction. Avoid Spring context.

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
  @Mock OrderRepository orders;

  private OrderService service;

  @BeforeEach
  void setUp() {
    service = new OrderService(orders);
  }

  @Test
  void createOrderRejectsBlankCustomerId() {
    assertThatThrownBy(() -> service.createOrder(new CreateOrderRequest(" ")))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessage("customerId is required");

    verifyNoInteractions(orders);
  }
}
```

## Spring Controller

Prefer direct invocation for controller units. Mock the service; do not start MVC or Spring.

```java
class OrderControllerTest {
  private final OrderService service = mock(OrderService.class);
  private final OrderController controller = new OrderController(service);

  @Test
  void createReturnsCreatedOrder() {
    var request = new CreateOrderRequest("customer-1");
    var response = new OrderResponse("order-1", "customer-1");
    when(service.createOrder(request)).thenReturn(response);

    var result = controller.create(request);

    assertThat(result.getStatusCode()).isEqualTo(HttpStatus.CREATED);
    assertThat(result.getBody()).isEqualTo(response);
  }
}
```

## Angular Service

Use a plain class test when Angular injection/rendering is not part of the behavior.

```ts
describe('CartService', () => {
  it('adds an item to the cart total', () => {
    const service = new CartService();

    service.add({ id: 'item-1', price: 25 });

    expect(service.total()).toBe(25);
  });
});
```

## Angular Component

Use `TestBed` only when rendering, bindings, outputs, DI, or lifecycle behavior matters.

```ts
describe('CounterComponent', () => {
  it('increments the visible count', async () => {
    await TestBed.configureTestingModule({
      imports: [CounterComponent],
    }).compileComponents();

    const fixture = TestBed.createComponent(CounterComponent);
    await fixture.whenStable();

    fixture.nativeElement.querySelector('button').click();
    await fixture.whenStable();

    expect(fixture.nativeElement.textContent).toContain('1');
  });
});
```

## Banned Patterns

- Spring: `@SpringBootTest`, `@WebMvcTest`, `@DataJpaTest`, `SpringExtension`, Testcontainers, repository integration tests, full application context tests.
- Infrastructure: real databases, queues, filesystems, HTTP clients, backend services, or network calls.
- Angular/E2E: Cypress, Playwright, real browser automation, real routing flows, real backend calls.
- General: tests that assert private methods, internal call order, framework wiring, or implementation details instead of observable behavior.
