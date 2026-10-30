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


## Precedence graph and recovery worked checks

Schedule: T1:R(A), T2:W(A), T1:W(B), T2:R(B). Conflicts on A give T1→T2 (T1 read before T2 write); conflicts on B give T2→T1 (T2 read before T1 write). Cycle means not conflict-serializable.

A lock schedule may deadlock: T1 holds A waits for B; T2 holds B waits for A. Wait-for graph has T1→T2 and T2→T1. Strict 2PL prevents other transactions from reading/writing uncommitted writes and simplifies rollback.

Write-ahead logging: before a dirty page reaches disk, its log record must be durable. REDO repeats committed updates after crash; UNDO reverses incomplete updates. Checkpoint records a recovery boundary but does not remove need for logs after it.

**Practice:** Build precedence graph for two schedules; identify dirty read; state whether strict 2PL gives conflict serializability; describe which transaction to undo after a crash.