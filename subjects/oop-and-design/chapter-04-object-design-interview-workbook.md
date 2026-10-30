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

## 5. Full worked object-design problem: parking garage

### Step 1: Clarify requirements

Ask: vehicle types? multiple floors? assign any suitable spot or nearest? reservations? hourly/daily pricing? lost tickets? payment failure? full garage behavior? concurrent entrances? For this design, assume cars and motorcycles, spots have types, ticket starts a visit, payment is due on exit, and concurrent requests cannot allocate one spot twice.

### Step 2: Identify concepts and responsibilities

- Garage owns floors and capacity summary.
- Floor owns spots and a search/index for available spots by type.
- ParkingSpot owns its identifier, type, and current occupancy.
- Vehicle has registration and category.
- Ticket binds one vehicle to one spot and records entry time/status.
- PricingPolicy calculates charge from timestamps and policy.
- PaymentGateway authorizes collection.
- GarageService coordinates operations and transaction boundaries.

Avoid putting every rule in one Garage class or allowing the UI to set spot occupancy directly.

### Step 3: State invariants

1. A spot is assigned to at most one active ticket.
2. An active ticket references exactly one occupied spot.
3. A closed ticket cannot be charged or closed again.
4. A vehicle has at most one active parking ticket, if policy requires it.
5. Payment failure does not free a spot until the exit policy completes successfully.

### Step 4: Sketch the model

~~~text
Garage 1 --- * Floor 1 --- * ParkingSpot
                               ^
Vehicle 1 --- * Ticket * ------+
                    |
                    +--- PricingPolicy
                    +--- PaymentGateway
~~~

The Ticket is an association object: it records data about the relationship between Vehicle and ParkingSpot over time.

### Step 5: Walk a use case

Entry: validate vehicle; in one transaction/atomic operation choose a compatible available spot, mark it occupied, create active ticket, and return ticket ID. If no spot exists, return a full-garage result. Concurrent entrances require locking/conditional update so two callers do not both claim the same spot.

Exit: load active ticket; calculate amount from entry time and pricing policy; authorize payment; if successful, close ticket and release spot atomically. Make payment/exit idempotent so a retry does not charge twice.

### Step 6: API and failures

~~~text
park(vehicle) -> Ticket | GarageFull | InvalidVehicle
quote(ticketId, exitTime) -> Money
exit(ticketId, paymentToken) -> Receipt | PaymentFailed | AlreadyClosed
~~~

Return structured errors; do not leak card data. Payment provider timeout is ambiguous: query provider/idempotency key before retrying a charge.

### Step 7: Testing

Unit: spot accepts compatible vehicle; pricing boundaries at exact hour; ticket cannot close twice.
Integration: transaction rolls back if ticket creation fails after spot selection.
Concurrency: two simultaneous requests for one remaining spot yield exactly one success.
Failure: payment timeout followed by retry results in one charge.
Policy: full garage returns clear result and leaves state unchanged.

### Interview presentation order

Start with clarifying questions, then core entities, invariants, API/use case, concurrency/failure handling, and extensions. Keep reservation, loyalty, multiple currencies, and analytics out until requirements call for them.

### Additional practice

1. Design a library checkout with holds and overdue fines. List invariants and state transitions.
2. Design a vending machine. Use states and explain what happens if an item is unavailable after payment.
3. Refactor a ShoppingCart that calculates discounts, persists orders, and formats HTML.
4. Write three concurrency tests for a single-seat ticket booking.
5. Compare a class diagram with a sequence diagram: which answers structure and which answers interaction?

**Answer guide:** The library needs Loan/Hold association records; vending machine needs explicit states and refund transitions; separate presentation/persistence from cart domain rules; booking needs atomic seat claim and idempotent payment. Class diagram shows types/relationships; sequence diagram shows messages over time.

**Notebook work:** Redraw the garage class diagram from memory, then produce the entry/exit sequence and annotate transaction boundaries.


## 6. Concurrency and durable state in the garage design

A common bug is a check-then-act race: two entrance gates both read “spot 12 is free,” then both assign it. The invariant “one active ticket owns a spot” must be enforced atomically. In one database, use a transaction plus a uniqueness constraint on active spot assignment, or lock the selected spot row. In a distributed design, use a reservation with an expiration and an idempotency key; do not assume an in-memory mutex coordinates multiple servers.

~~~text
Available -> Reserved(ticket, expiry) -> Occupied
      ^               |                    |
      | timeout       | vehicle enters     | payment/exit succeeds
      +---------------+                    v
                                      Released
~~~

Keep separate the physical spot state and payment state. A payment timeout is ambiguous: the provider may have charged the customer even when the garage did not receive the response. Reuse the same provider idempotency key when retrying and reconcile status before issuing another charge. Ticket completion should be idempotent so an exit retry does not charge twice or free another vehicle's spot.

### API and consistency choices

- Search availability: may be eventually consistent for display, but assignment must be authoritative and atomic.
- Reserve spot: return a reservation/ticket identifier and expiry.
- Enter: transition only a matching, unexpired reservation to occupied.
- Pay/exit: record payment outcome, close the ticket, and release its spot in a controlled workflow.
- Admin override: require authorization, reason, audit record, and safe recovery path.

### Interview trade-offs

A single relational database is a strong starting point: it supports transactions, constraints, and straightforward reporting. Add a cache for read-heavy availability only after measuring; cached counts may be stale, so never use them as the final allocation decision. Partition by garage if scale requires it, because most operations are local to one facility.

Discuss failure cases explicitly: gate offline, sensor disagrees with database, lost ticket, payment provider timeout, duplicate scan, and manual closure of a spot. Define reconciliation and operator workflow; “add a microservice” does not resolve inconsistent physical and digital state.

### Final practice prompt

Design a reservation expiration process. State who is allowed to release a reservation, how clock skew is handled, whether a late-arriving vehicle can still enter, what idempotency key identifies retries, and what event/audit record is written. A good answer states policy before proposing a timer or queue.
