# Chapter 3 — Responsibility, SOLID, and Design Patterns

## 1. Cohesion and coupling
Cohesion asks whether a module's responsibilities belong together; coupling measures dependencies between modules. Aim for focused responsibilities and explicit boundaries. Avoid both giant classes and needless fragmentation.

SOLID:
- Single Responsibility: one reason to change.
- Open/Closed: extend behavior without editing stable core unnecessarily.
- Liskov Substitution: subtypes preserve contracts.
- Interface Segregation: clients should not depend on unused methods.
- Dependency Inversion: high-level policy depends on abstractions.

Treat these as design heuristics, not rigid laws.

## 2. Patterns
Strategy encapsulates interchangeable algorithms. Factory centralizes object creation. Observer notifies subscribers of state changes. Adapter translates one interface to another. Decorator adds behavior through wrapping. Use patterns to solve recurring coupling/creation problems; avoid applying them where a simple function is clearer.

## 3. Worked design
For notifications, define NotificationSender interface; EmailSender and SmsSender implement it. A service depends on the abstraction, so tests can inject a fake sender. Keep retry policy separate if it changes independently.

## 4. Questions and practice
**Q1.** Strategy is useful when? **A:** Choose algorithm at runtime without a large conditional.
**Q2.** What does dependency inversion improve? **A:** High-level policy is insulated from concrete details.
**Q3.** Is a pattern automatically better? **A:** No; it must reduce a real design problem.
Practice: select a pattern for interchangeable discounts; split a God class; design a test seam.

## Notebook work
Draw the notification interface and its implementations. State one trade-off of the abstraction.


## Pattern trade-off example

Without Strategy, a checkout service may have a long conditional for card, wallet, and bank transfer. Strategy extracts a PaymentMethod interface and implementations. Benefit: add behavior without editing the checkout algorithm. Cost: more types and object wiring; for two stable branches, a simple conditional may be clearer.

Observer decouples event producer from subscribers, but raises questions: ordering, exceptions, duplicate delivery, and unsubscribing. Factory centralizes construction when creation is complex; it is unnecessary for a trivial constructor.

SOLID quick check: if changing tax rules edits inventory storage code, responsibilities are mixed. If a client depends on a huge interface but uses one method, split a focused interface. If a high-level service constructs its concrete database adapter internally, inject an abstraction for testability.

**Practice:** Select Strategy for compression options; Adapter for a legacy API; Decorator for adding logging. Explain one downside for each rather than naming a pattern alone.

## 5. Cohesion, coupling, and the SOLID principles

Cohesion asks whether the responsibilities inside one module belong together. Coupling asks how strongly modules depend on one another. A design with focused modules and explicit dependencies is easier to change, test, and explain.

**Single Responsibility:** one module has one coherent reason to change. A Receipt that calculates tax, formats HTML, writes files, and sends email has unrelated change axes. Separate calculation from presentation and delivery.

**Open/Closed:** extend behavior through a stable seam when variation is expected. It does not mean never edit existing code; a complicated plugin system for two fixed cases can be worse than a small conditional.

**Liskov Substitution:** subtypes preserve expectations and contracts, as detailed in Chapter 2.

**Interface Segregation:** clients should depend on small contracts they use. Split a huge PrinterScannerFax interface if some devices cannot fax.

**Dependency Inversion:** high-level policy depends on abstractions, not concrete infrastructure. The domain service can accept a PaymentGateway interface; production injects a provider adapter and tests inject a fake.

These are design heuristics. Apply them to a concrete change request and weigh the costs.

## 6. Patterns as collaboration structures

**Strategy:** context delegates one varying algorithm to a strategy object. Example: ShippingCostPolicy has Standard and Express implementations. Client selects policy; context remains stable.

**Factory:** centralizes a complex or environment-dependent construction choice. Factory is unnecessary when a constructor is already clear.

**Observer:** publisher notifies subscribers about an event. Define ordering, failure isolation, subscription lifetime, and duplicate delivery. In distributed systems, observers often become event consumers with retry/idempotency concerns.

**Adapter:** wraps a legacy or external API and translates it to the application's interface. Keep vendor-specific types behind the adapter.

**Decorator:** wraps an object implementing the same interface and adds behavior such as logging or caching. Order of decorators can affect behavior.

**Repository:** abstracts persistence operations around domain concepts. Avoid making it a generic CRUD wrapper that hides important query semantics.

### Pattern selection example

Requirement: calculate shipping cost by selected delivery option. Start with a function or conditional if there are only two stable cases. If options vary by region and must be tested/extended independently, Strategy can isolate each policy. Name the variation point and explain the cost of extra types.

~~~text
Checkout -> ShippingPolicy
                |-- StandardPolicy
                |-- ExpressPolicy
                +-- PickupPolicy
~~~

### Testing and patterns

Inject clock, random source, network gateway, and storage when deterministic tests require control. Do not create interfaces for every class automatically. An abstraction is useful when it protects a boundary, supports multiple implementations, or gives a meaningful test seam.

### Common design smells

- God object with many unrelated responsibilities.
- Feature envy: one class repeatedly reads another's internals to perform that object's work.
- Shotgun surgery: one change requires edits across many modules.
- Primitive obsession: domain concepts represented as loosely interpreted strings/integers.
- Boolean parameter switches behavior in a confusing way.
- Premature abstraction creates more indirection than variation.

Refactor only after understanding behavior and preserving tests/contracts.

### Questions and answers

**Q1.** Does dependency inversion require a framework?  
**Answer:** No. Passing an interface or function into a service is enough.

**Q2.** What is the main Strategy trade-off?  
**Answer:** It reduces conditional coupling and supports variation, but adds objects/wiring.

**Q3.** What can go wrong with Observer?  
**Answer:** Hidden control flow, notification ordering, subscriber failures, leaks, and duplicate events.

**Q4.** Why is a Repository not always a generic save/load interface?  
**Answer:** Queries and transaction semantics may be domain-specific; hiding them can make behavior unclear.

### Practice with answer guide

1. A class both calculates invoice totals and sends email. Identify responsibilities to extract.
2. A service hardcodes a payment provider. Introduce a useful abstraction boundary.
3. Choose Adapter for a vendor API whose method names/types differ from internal conventions.
4. Explain why a Strategy is unnecessary for one unchanging calculation.
5. Draw a Decorator stack for metrics, logging, and retry around a network client. State why order matters.

**Notebook exercise:** For each SOLID principle, write a one-sentence rule, a violating design, and a repair. Do not memorize only the acronym.


## 7. Work through SOLID on an order workflow

Imagine OrderService validates a cart, calculates tax, charges a card, writes a database row, sends email, and formats a receipt. A change to tax rules risks breaking email; every test requires payment and database setup. This is a cohesion problem. Split responsibilities along reasons to change: pricing policy, payment boundary, order persistence, notification, and orchestration.

- **Single Responsibility:** one module has one coherent reason to change; it does not mean one method per class.
- **Open/Closed:** add a new pricing policy by implementing a contract rather than editing a long conditional in many places.
- **Liskov Substitution:** every PaymentProcessor implementation honors the same accepted inputs and failure/result contract.
- **Interface Segregation:** a read-only reporting client should not implement unrelated update methods just because one huge interface requires them.
- **Dependency Inversion:** high-level checkout flow depends on abstractions; infrastructure adapters implement them.

SOLID is a set of design heuristics, not a command to create five interfaces in every small program. Abstractions are useful when they isolate a volatile boundary, enable meaningful substitution, or simplify testing. An abstraction with only one implementation and no expected variation can add needless indirection.

### Choose patterns by the problem they solve

| Need | Candidate pattern | Cost to watch |
|---|---|---|
| Select one of several interchangeable algorithms | Strategy | Extra objects/configuration |
| Create objects without exposing construction details | Factory | Indirection; avoid if constructor is clear |
| Add behavior around an object | Decorator | Wrapping order can affect behavior |
| Notify multiple interested subscribers | Observer | Lifetime, ordering, and error handling |
| Translate an incompatible external API | Adapter | Mapping and semantic mismatch |
| Build a complex object step by step | Builder | Boilerplate for simple objects |

For discounts, Strategy can model percentage, fixed amount, or loyalty pricing. For logging around a payment adapter, Decorator can add timing without changing payment logic. Do not choose a pattern because its name sounds “enterprise”; explain the pressure it resolves and the alternative.

### Testing design seams

A useful seam is a place where a test can replace a slow or nondeterministic dependency. Inject a clock to test expiration, a repository to test business rules, and a payment gateway to simulate timeout. Avoid a mock for every internal call; tests should assert externally meaningful behavior, not freeze incidental implementation.

### Design-smell exercise

If a class has many unrelated reasons to change, split by responsibility. If adding a subtype requires modifying every caller's type switch, consider polymorphism or a strategy. If a subclass overrides methods to throw “unsupported,” the parent contract is probably too broad or the subtype is invalid.
