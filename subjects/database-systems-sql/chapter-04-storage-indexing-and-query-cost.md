# Chapter 4 — Storage, Indexes, and Query Cost

## 1. Pages and records
Databases transfer pages between storage and memory. Blocking factor depends on page and record size. Sequential scans read pages linearly; indexes add storage and maintenance cost but reduce lookup I/O.

## 2. B+ tree index
B+ tree internal nodes guide search; records or record pointers are stored at linked leaves. High fanout keeps height small. Search, insertion, and deletion take O(log_f N) node visits, with splits/merges maintaining occupancy.

Clustered index orders data pages by key; one such physical ordering is typical. Secondary indexes add lookup paths. Range queries benefit from ordered leaves.

## 3. Hash index and trade-offs
Hash indexes support equality lookup well but not range order. B+ trees support equality and ranges. Indexing every column is costly: writes update indexes, pages consume space, and optimizer choice depends on selectivity.

## 4. Query planning
Optimizer estimates cost using table size, selectivity, indexes, and join algorithms. Nested-loop join can be good for small input/indexed lookup; hash join suits equijoins; sort-merge join exploits sorted inputs. Push selective filters early when semantics allow.

## 5. Questions and practice
**Q1.** Which index supports range scan? **A:** B+ tree.
**Q2.** Which is useful for equality but not range? **A:** Hash index.
**Q3.** Why can too many indexes hurt? **A:** Storage and write-maintenance cost.
Practice: choose index for a query; estimate pages scanned by a full scan; compare nested-loop and hash join.

## Notebook work
Draw B+ tree internal nodes and linked leaves; trace a range lookup.


## B+ tree and I/O cost intuition

A B+ tree of order/fanout f has high branching, so height is about log_f(N); with large f, a table with millions of records can be reached in a few page reads. Internal nodes contain separator keys and child pointers; leaves contain ordered keys and record pointers/data and are linked for range scans.

Search follows one root-to-leaf path. Insert into leaf; if full, split and promote separator; splits can cascade to root. Deletion may borrow from sibling or merge to restore minimum occupancy.

B-tree may store records/record references in internal and leaf nodes; B+ tree stores data entries at leaves and separators internally. Both are balanced multiway search trees; use the exact convention given in a problem.

If each page holds 100 entries, fanout around 100 gives a rough height: root → internal → leaf can cover about 1 million leaves/records (100×100×100). Real occupancy and keys change exact counts.

**Practice:** Trace insertion causing a split; explain why leaf links make range scans efficient; choose B+ tree vs hash index for equality and range queries.