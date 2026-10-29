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
