# Chapter 5 — Transactions, Concurrency, and Recovery

## 1. ACID
Atomicity: all-or-nothing. Consistency: constraints hold across commit. Isolation: concurrent execution behaves according to a chosen isolation level. Durability: committed changes survive failures.

A schedule interleaves read/write operations. Conflict serializability: build precedence graph with transaction nodes and edges for conflicting operations on same item; acyclic graph implies conflict-serializable schedule.

## 2. Locks and anomalies
Shared locks permit readers; exclusive locks protect writers. Two-phase locking has growing phase then shrinking phase and guarantees conflict serializability. Strict 2PL holds write locks to commit, simplifying recovery.

Isolation phenomena include dirty read, non-repeatable read, and phantom read. MVCC stores versions to let readers see a consistent snapshot; serialization and locking details depend on DBMS.

## 3. Deadlocks and recovery
Wait-for graph cycle indicates deadlock. Detect, abort a victim, and retry; or prevent with ordering/timeouts. Write-ahead logging requires log record durable before modified page reaches disk. Checkpoints limit recovery work. Undo reverses incomplete transactions; redo reapplies committed updates.

## 4. Questions and practice
**Q1.** What does durability promise? **A:** Committed updates survive failures.
**Q2.** How test conflict serializability? **A:** Precedence graph acyclic.
**Q3.** Why write-ahead log? **A:** Recovery information is durable before data-page writes.
Practice: draw a precedence graph; identify dirty read; trace undo/redo after crash; explain strict 2PL.

## Notebook work
Make a two-transaction schedule and draw its conflict graph.
