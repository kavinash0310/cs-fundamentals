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

## Isolation anomalies and serializability

Dirty read: T2 reads T1's uncommitted value; if T1 aborts, T2 used data that never committed. Non-repeatable read: the same row returns a different committed value on a second read. Phantom: repeating a predicate query returns added/removed rows.

Conflict graph edges follow operation order for same-item conflicting operations (RW, WR, WW across different transactions). Acyclic graph means conflict serializable; a cycle means not conflict serializable. View serializability is broader and harder to test.

Strict two-phase locking: acquire locks before access, do not release early, and hold write locks until commit/abort. It ensures conflict serializability and prevents cascading aborts, but can deadlock.

Recovery example: T1 writes A and commits; T2 writes B but has not committed when crash occurs. Recovery redoes T1 if its update is missing from data page and undoes T2's uncommitted change, using the log and checkpoint rules.

**Practice:** Identify dirty/non-repeatable/phantom reads from schedules; build a precedence graph; explain why serializable isolation may reduce concurrency.

## Extended lecture: schedules, locks, and restart recovery

A transaction should preserve consistency while concurrent operations interleave. A schedule is conflict-serializable if its precedence graph is acyclic. Create one node per transaction; for every conflicting pair on the same item (at least one write), add an edge from the earlier operation's transaction to the later one's. A cycle means the schedule is not conflict-serializable.

**Worked graph.** T1 does R(A), W(A), R(B); T2 does R(B), W(B), R(A). If T1's W(A) occurs before T2's R(A), add T1→T2. If T2's W(B) occurs before T1's R(B), add T2→T1. The cycle means no serial ordering can preserve both conflicts. Conflict means same data item and at least one write; read-read does not create an edge.

### Strict two-phase locking

In 2PL, a transaction first acquires locks (growing phase), then releases locks (shrinking phase); after it releases one lock, it cannot acquire another. Strict 2PL holds exclusive locks until commit/abort, preventing other transactions from reading uncommitted writes and simplifying recovery. Deadlocks remain possible: T1 holds A and waits for B while T2 holds B and waits for A. A wait-for graph cycle detects deadlock; the system chooses a victim to roll back.

### WAL recovery

Write-ahead logging requires the log record describing an update to reach stable storage before the changed data page is written. A commit record must be durable before the DBMS reports commit success. After a crash, recovery identifies transactions and redoes committed updates that may not have reached data pages; it undoes incomplete transactions whose writes may have reached disk. Exact algorithms may use checkpoints and pageLSNs to limit work.

~~~text
log: <T1 start>
     <T1, A, old=5, new=9>
     <T1 commit>   <-- durable commit
data page A written later
~~~

A checkpoint records a recovery boundary, but it does not necessarily force every dirty page to disk. WAL supports atomicity and durability; isolation is handled by concurrency control and consistency by constraints/application logic.

### Isolation anomalies

- Dirty read: observe another transaction's uncommitted write.
- Nonrepeatable read: same row changes between reads.
- Phantom: a repeated predicate query gains or loses matching rows.
- Lost update: concurrent writes overwrite one another.

Isolation-level names and exact phantom behavior vary across DBMS implementations. State the anomaly and mechanism rather than relying only on a label.

### Review

1. Is a schedule with an acyclic precedence graph conflict-serializable? Yes.
2. Does strict 2PL prevent every deadlock? No; it can create deadlocks.
3. What must reach stable storage before a dirty page under WAL? The corresponding log record.
4. What is redo for? Reapply an update from a committed transaction when necessary after a crash.
