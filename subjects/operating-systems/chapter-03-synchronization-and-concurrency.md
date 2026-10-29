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
