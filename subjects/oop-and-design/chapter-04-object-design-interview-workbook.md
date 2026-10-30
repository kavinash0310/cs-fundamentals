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


## Worked design: library checkout

Requirements: copy of a book may be checked out by at most one active loan; member may not exceed a limit; return closes loan and may calculate overdue fee.

Objects: BookTitle describes catalog metadata; Copy has copy ID/status; Member has member ID; Loan records member, copy, checkout/due/return times; LibraryService coordinates policy.

Invariants: an active Loan references exactly one checked-out Copy; a returned copy has no active loan. Use a clock interface for deterministic due-date tests. Concurrent checkout needs an atomic reservation/transaction so two requests cannot both claim the same copy.

Tests: checkout available copy succeeds; second concurrent checkout fails; overdue return calculates fee; invalid member is rejected; return twice is idempotent or reports a clear error.

**Interview presentation:** clarify holds, renewals, fines, multiple branches, persistence, and concurrency before implementing every feature. Draw a small class diagram, state each responsibility, then walk through one use case.