# Chapter 4 — Object Design Interview Workbook

## 1. Design process
Clarify requirements and edge cases; identify domain objects; assign responsibilities; define APIs and invariants; sketch relationships; discuss persistence/concurrency only when relevant; test with scenarios.

Example parking lot: Vehicle, Spot, Ticket, Entrance, Payment. Requirements decide whether spots have types, reservations, pricing, and multiple floors. Avoid designing features not requested.

## 2. State and behavior
Model transitions explicitly: ticket issued → paid → closed. Prevent invalid transitions in the object responsible for ticket lifecycle. Prefer immutable value objects for money/coordinates where appropriate.

## 3. Testing
Unit-test behavior and boundaries, not private implementation. Test invalid operations, equality/hash contracts, and substitutability. Use dependency injection for clocks, payment providers, or network clients.

## 4. Questions and practice
**Q1.** What is first in a design interview? **A:** Clarify scope and constraints.
**Q2.** Where should an invariant live? **A:** With the object/module that owns that state.
**Practice:** model a library checkout, elevator controller, or shopping cart; list requirements, APIs, states, and three tests.

## Notebook work
Draw a class diagram for the chosen problem, then explain why each class owns its data.
