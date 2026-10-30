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


## Capacity and reliability calculations

Estimate daily storage: 1 million events/day × 1 KB/event ≈ 1 GB/day raw; indexes, replicas, metadata, and retention multiply it. Estimate peak QPS separately from daily average. State assumptions and units.

Availability composition: if two independent serial components each have 99.9% availability, combined availability is 0.999²≈99.8001%. Redundant parallel components can improve availability only when failover and dependencies are independent.

Caching requires correct invalidation. If user permissions can change, cached results must include authorization context or be revalidated. A cache hit is not a reason to skip access control.

CAP context: during a network partition, a distributed system cannot guarantee both every operation completes and all replicas immediately agree. Many real systems choose behavior per operation and recover consistency later.

**Practice:** Estimate storage for 5M records × 2KB; calculate monthly downtime budget for 99.9%; identify a read bottleneck and a write bottleneck.

## Extended lesson: size a service and protect it under load

Suppose an endpoint receives 2,000 requests/second and each request consumes 20 ms of CPU time. CPU demand is about 2,000×0.020=40 CPU-seconds per second, or roughly 40 fully occupied cores before headroom, I/O waits, and uneven load. If target utilization is 60%, a first estimate is 40/0.6≈67 cores. This is a capacity estimate, not a benchmark; measure with representative workloads.

Horizontal scaling adds instances; it works best when request handling is stateless or state is externalized. A shared database may become the bottleneck. Caches reduce repeated reads but introduce freshness/invalidation questions. Sharding partitions data to distribute load but complicates cross-shard queries and transactions.

### Availability and failure handling

Availability is often expressed as uptime fraction. 99.9% over a 30-day month allows about 43.2 minutes of downtime; 99.99% allows about 4.32 minutes. Retries help transient failures but can amplify overload. Use deadlines, bounded retries with backoff/jitter, circuit breakers, and load shedding. Idempotency is essential when a caller cannot tell whether an operation completed before a timeout.

CAP is about behavior during a network partition: a distributed system cannot guarantee both strong consistency and availability for every request in that partition. It does not say every database permanently chooses only two of three labels. Specify which operations may return stale data and which must fail or wait.

### Token-bucket rate limiter

Bucket holds up to B tokens and refills at r tokens/second. Each request consumes one token; if none remain, reject or queue. With B=10 and r=2/s, a client may send a burst of 10 immediately and then sustain about 2/s. In a distributed deployment, shared counters/atomic updates or deliberate per-instance limits are needed.

~~~text
refill r tokens/s -> [ bucket, capacity B ] -> request consumes 1
                                   |
                              empty: deny/429
~~~

### Review questions

- Why use backoff with jitter? To avoid synchronized retry storms.
- What is a cache-invalidation risk? Stale or inconsistent data after updates.
- Does adding app servers always increase throughput? No; shared dependencies may bottleneck.
