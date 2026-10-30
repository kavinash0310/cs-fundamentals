# Chapter 4 — Deadlocks

## 1. Four necessary conditions
Deadlock requires all four conditions:
1. Mutual exclusion: resource is non-shareable.
2. Hold and wait: process holds resources while requesting more.
3. No preemption: resources are not forcibly removed.
4. Circular wait: a cycle of processes each waits for another.

A resource-allocation graph uses process and resource nodes. A cycle is necessary for deadlock; with one instance per resource type, cycle is sufficient. With multiple instances, a cycle alone may not prove deadlock.

## 2. Strategies
- Prevention: break at least one necessary condition (e.g., global lock ordering breaks circular wait).
- Avoidance: grant requests only if resulting state is safe. Banker's algorithm checks whether some process completion sequence can release enough resources for all.
- Detection/recovery: allow deadlocks, detect cycles/unsafe waits, then terminate, rollback, or preempt with care.
- Ignore: practical when risk/cost is low, but not a correctness strategy for every system.

Safe state means some completion order exists; unsafe does not mean deadlocked yet.

## 3. Questions and practice
**Q1.** Name all conditions. **A:** Mutual exclusion, hold-and-wait, no preemption, circular wait.
**Q2.** Does unsafe imply deadlocked? **A:** No.
**Q3.** A cycle with one instance per resource type? **A:** Deadlock.
Practice: draw a two-lock cycle; show how lock ordering prevents it; test a small Banker's safe sequence.

## Notebook work
Draw a resource allocation graph and annotate request versus assignment edges.


## Banker's safety check example

Available resources A,B,C = (3,3,2). A process is finishable when Need≤Available; then release its Allocation.

| Process | Allocation | Need |
|---|---|---|
| P0 | (0,1,0) | (7,4,3) |
| P1 | (2,0,0) | (1,2,2) |
| P2 | (3,0,2) | (6,0,0) |
| P3 | (2,1,1) | (0,1,1) |
| P4 | (0,0,2) | (4,3,1) |

With available (3,3,2), P1 can finish, release (2,0,0) → (5,3,2). Then P3 can finish → (7,4,3); P4 can finish → (7,4,5); P0 can finish → (7,5,5); P2 can finish. A safe sequence is P1,P3,P4,P0,P2.

Need=Max-Allocation. A request first must be ≤ Need and ≤ Available; tentatively allocate, then run safety test. A safe state has at least one completion sequence; unsafe does not automatically mean deadlock.

**Practice:** Name the four deadlock conditions; explain why global lock ordering prevents circular wait; distinguish prevention from avoidance.

## Resource graph and deadlock-avoidance details

In a resource-allocation graph, request edge P→R means process P waits for resource R; assignment edge R→P means an instance is held by P. For one instance of each resource type, a cycle implies deadlock. With multiple instances, a cycle may only indicate the possibility of deadlock.

Deadlock avoidance examines maximum claims before granting. Need=Max-Allocation; Available=Total-ΣAllocation. Safety algorithm repeatedly finds a process whose remaining Need fits Available, simulates completion, and returns its allocation. If all can finish, the state is safe.

Request example: Available=(2,1). P0 Need=(1,1), P1 Need=(2,0). Granting P0 request (1,0) leaves (1,1); P0 can finish and release resources, then P1 can finish. The request preserves safety. A request greater than Need is invalid; greater than Available must wait.

Recovery trade-offs: abort all deadlocked processes (simple but wasteful), abort one at a time, or preempt resources. Repeatedly choosing the same victim can starve; include rollback cost and progress in victim choice.

## Extended lesson: apply Banker's safety test

Deadlock requires all four Coffman conditions: mutual exclusion, hold-and-wait, no preemption, and circular wait. Prevention breaks at least one condition. Avoidance instead examines each request and grants it only if the resulting state remains safe. Detection allows deadlock and later finds it; recovery may abort a process or preempt resources.

In Banker's algorithm, Need=Max−Allocation. A state is safe if there exists an ordering in which every process can obtain its remaining Need from currently available resources plus resources released by earlier completed processes.

Use this standard snapshot; vectors are resource types A,B,C:

| Process | Allocation | Max | Need |
|---|---|---|---|
| P0 | (0,1,0) | (7,5,3) | (7,4,3) |
| P1 | (2,0,0) | (3,2,2) | (1,2,2) |
| P2 | (3,0,2) | (9,0,2) | (6,0,0) |
| P3 | (2,1,1) | (2,2,2) | (0,1,1) |
| P4 | (0,0,2) | (4,3,3) | (4,3,1) |

Available=(3,3,2). Start Work=(3,3,2). P1 fits because (1,2,2)≤Work; after it finishes, release allocation (2,0,0), so Work=(5,3,2). P3 then fits and releases (2,1,1): Work=(7,4,3). P4 fits and releases (0,0,2): Work=(7,4,5). P0 fits and releases (0,1,0): Work=(7,5,5). P2 fits. Safe sequence: P1,P3,P4,P0,P2. A safe sequence is a certificate that completion is possible; it is not necessarily the only one.

### Resource-allocation graph and recovery

For single-instance resources, a cycle in the wait-for graph indicates deadlock. With multiple instances, a cycle alone is not sufficient. Recovery may terminate one process, roll it back to a checkpoint, or preempt a resource if its state can be restored. Victim choice should consider lost work, priority, and repeated rollback risk.

### Questions

- Is an unsafe state already deadlocked? Not necessarily; it means no safe completion order can be guaranteed.
- Which condition does lock ordering prevent? Circular wait.
- How does Banker differ from prevention? It grants requests only when the resulting state remains safe.
