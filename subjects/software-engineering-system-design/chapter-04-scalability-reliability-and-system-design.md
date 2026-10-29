# Chapter 4 — Scalability, Reliability, and System Design

## 1. Clarify first
For a design prompt, ask about users, request rate, data size, latency, availability, consistency, security, cost, and failure behavior. State assumptions when unknown.

## 2. Capacity and scaling
Vertical scaling increases one machine's capacity; horizontal scaling adds machines. Stateless services scale more easily behind a load balancer. A bottleneck may be CPU, memory, disk, network, database, or a downstream dependency.

Caching stores reusable results near consumers. Define cache key, TTL/invalidation, stale-data policy, and stampede protection. Queues smooth bursts but require retries, dead-letter handling, and idempotent consumers.

## 3. Reliability
Availability is fraction of time a service is usable. Redundancy helps only if failures are independent and failover works. Timeouts prevent indefinite waits; retries need backoff/jitter and limits; circuit breakers reduce overload propagation. Observability uses logs, metrics, traces, and useful alerts.

## 4. Consistency and trade-offs
A distributed write may be acknowledged before replicas converge. Choose strong or eventual consistency from user requirements. Partition tolerance and network failures force trade-offs in distributed systems; do not apply CAP as a slogan without identifying a partition and the operations affected.

## 5. Design outline
~~~text
Clients -> Load balancer -> Stateless API -> Cache
                                |              |
                                +-> Database <-+
                                +-> Queue -> Workers
~~~
Explain data ownership, hot keys, database indexes, backups, authorization, deployment, and failure modes. Grow complexity only when scale or requirements justify it.

## 6. Questions and practice
**Q1.** Why are retries dangerous without idempotency? **A:** They can duplicate side effects.
**Q2.** How handle cache stampede? **A:** Coalesce refreshes, jitter TTLs, or serve stale while refreshing.
**Q3.** What does a circuit breaker do? **A:** Stops repeated calls to failing dependency temporarily.
Practice: design URL shortener or rate limiter; estimate QPS/storage; identify single points of failure and data consistency needs.

## Notebook work
For one design prompt, show request flow, data model, scaling bottleneck, and two failure scenarios.
