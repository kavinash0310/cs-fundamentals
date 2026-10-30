# Chapter 2 — Modularity, Architecture, and APIs

## 1. Modules and boundaries
A module groups behavior behind an interface. High cohesion keeps related responsibilities together; low coupling limits ripple effects. Layered architecture separates presentation, domain, and persistence. Hexagonal architecture places domain logic behind ports/adapters. Microservices separate deployable services but add network and operational cost.

Choose boundaries based on ownership, change patterns, data consistency, and team size; do not split a monolith by fashion.

## 2. APIs
An API defines requests, responses, errors, authentication, versioning, and compatibility. Design idempotency for retryable writes: repeating the same idempotency key should not create duplicate effects.

Pagination, filtering, timeouts, rate limits, and validation should be explicit. Avoid leaking internal database models as permanent public contracts.

## 3. Data modeling
Choose entities and relationships from domain behavior. Define ownership, constraints, and lifecycle. Normalize to avoid anomalies, then denormalize only for measured read needs. Specify consistency expectations.

## 4. Diagrams
~~~text
Client -> API -> Domain service -> Repository -> Database
                    |
                    +-> Event/queue -> Worker
~~~
Synchronous calls return immediately but couple latency; queues decouple workloads but add eventual consistency and retry/idempotency needs.

## 5. Questions and practice
**Q1.** What does an adapter do? **A:** Translates an external interface to an internal port.
**Q2.** What is idempotency useful for? **A:** Safe retries of operations.
**Q3.** What cost do microservices add? **A:** Network, deployment, observability, and data-consistency complexity.
Practice: design API for order creation; state error semantics; draw a module boundary and its dependencies.

## Notebook work
Draw the data and control flow for one request, including failure boundaries.


## API and architecture worked example

Create-order endpoint: validate cart and authorization; accept an idempotency key; atomically reserve/order records; return order ID and status. A repeated request with same key returns the same logical result. Use 4xx for caller-correctable errors and 5xx for server/dependency failures, with stable error codes and no sensitive internals.

A monolith can still have modular boundaries and one deployment. A service split is justified when independent scaling/ownership/deployment outweighs network calls, duplicated data, and operational load. A queue is appropriate for slow email generation; an order confirmation must not be lost, so persist an outbox event in the same database transaction and publish it reliably.

**Practice:** Design a paginated list endpoint; define cursor stability; identify which service owns a user record; explain how to version an API without breaking old clients.