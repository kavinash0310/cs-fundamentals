# Chapter 3 — Synchronization and Concurrency

## 1. Race conditions and critical sections
A race occurs when result depends on uncontrolled interleaving. A critical section accesses shared state. Correctness goals: mutual exclusion, progress, and bounded waiting.

Example: x=x+1 is read, add, write; two threads can both read 0 and both write 1, losing an increment.

## 2. Locks, semaphores, monitors
A mutex grants one owner access; always release it and avoid inconsistent lock order. A semaphore has integer state and atomic wait/signal operations. Binary semaphores can protect a critical section; counting semaphores represent multiple resources.

A monitor groups shared data and operations; condition variables allow waiting for a predicate. Wait atomically releases the monitor lock and blocks; signal wakes a waiter, which must recheck its condition.

~~~text
lock(m)
while buffer is full:
    wait(notFull)
insert(item)
signal(notEmpty)
unlock(m)
~~~
Use while, not if, because wakeups may be spurious or another thread may consume the condition first.

## 3. Producer-consumer example
A bounded buffer of capacity N needs mutex plus empty=N, full=0 semaphores. Producer waits empty, locks, inserts, unlocks, signals full. Consumer waits full, locks, removes, unlocks, signals empty. Order matters to prevent deadlock.

## 4. Questions and practice
**Q1.** Why must x++ be synchronized? **A:** It is not atomic as a whole.
**Q2.** Mutex vs counting semaphore? **A:** Ownership lock versus a count of available permits/resources.
**Q3.** Why use a while around condition wait? **A:** Recheck invariant after wake.
Practice: find a lost-update interleaving; outline bounded-buffer operations; explain deadlock from opposite lock order.

## Notebook work
Draw two thread timelines showing a race, then add a mutex and show the corrected interleaving.


## Semaphore ordering and invariant

For a bounded buffer of capacity N, initialize empty=N, full=0, mutex=1:
~~~text
producer: wait(empty); wait(mutex); insert; signal(mutex); signal(full)
consumer: wait(full); wait(mutex); remove; signal(mutex); signal(empty)
~~~
The semaphore counts are invariants: 0≤full≤N, 0≤empty≤N, full+empty=N when no operation is in its critical update. Acquiring empty/full before mutex prevents a thread from holding the mutex while waiting for space/data.

Peterson's algorithm for two processes uses intent flags and turn; under assumptions of atomic reads/writes and memory ordering, it demonstrates mutual exclusion, progress, and bounded waiting. Modern CPUs/compiler reorderings require fences/atomic primitives; do not use textbook code as a production lock.

Condition-variable pattern is lock, while predicate false wait, update predicate, signal/broadcast, unlock. Always protect the predicate with the same lock.

**Practice:** Show the lost update in x=x+1; find why reversing semaphore order can deadlock; distinguish semaphore resource count from mutex ownership.

## Critical-section reasoning

A mutex protects a critical section, but it does not make arbitrary larger workflows atomic. Lock only the state needed, use a consistent global lock order, and avoid calling unknown callbacks while holding a lock.

Binary semaphore initialized 1 can behave like a lock, but a semaphore has no inherent owner; one thread can signal a permit another acquired. A mutex typically has ownership semantics.

Peterson's algorithm uses flag[0], flag[1], and turn. Each process raises its intent, gives priority to the other, and spins while the other wants entry and has priority. Under the sequentially consistent atomic-memory model it satisfies mutual exclusion, progress, and bounded waiting for two processes. Real compiler/CPU reordering means production code needs language atomics or OS primitives.

Condition-variable example: while count==0, wait(notEmpty); then remove item. The monitor lock is released while waiting and reacquired before return. Rechecking the predicate protects against spurious wakeups and competing consumers.

**Reasoning drill:** If two locks protect A and B, and T1 acquires A then waits for B while T2 acquires B then waits for A, circular wait occurs. A consistent order (always A before B) removes this cycle.