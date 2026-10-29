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
