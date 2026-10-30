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

## Storage and query-cost reasoning

A heap file stores records without key order; scans are simple but equality lookup may read every page. Sorted files support ordered/range access but insertion may require shifting/reorganization. Hash file organization maps key to bucket and suits equality lookup, not ranges.

Index is sparse if it has entries for blocks/ranges, dense if it has entries for every search key/record (definitions depend on indexed file organization). Clustered index orders data pages by search key; secondary index points to records in another physical order. A range query on a clustered index often reads fewer scattered pages.

Cost is often estimated in page I/Os rather than CPU comparisons. A B+ tree equality lookup follows root-to-leaf pages; a range lookup adds sequential leaf pages. Hash join partitions/probes relations; nested-loop join can be efficient when outer input is small and inner has index.

**Practice:** Choose heap, sorted, hash, or B+ tree organization for workloads; estimate pages for a full scan; explain why an index may not help when a query returns most rows.

## Extended lecture: estimate query cost

A database stores pages, not abstract rows. If a file has B pages and an unclustered selection returns many rows, the engine may perform many random I/Os even when the index lookup itself is cheap. A clustered index stores nearby key values in nearby pages, which helps range scans; a secondary/unclustered index can require one data-page access per matching tuple.

A B+ tree keeps search keys in internal pages and data entries or row pointers in leaves. All leaves are at the same depth and are linked for ordered range scans. A lookup follows one root-to-leaf path; insertion can split a full node and propagate a separator upward. In a high-fanout tree, height remains small, so point lookup is roughly a few page I/Os.

### Join choice depends on data size and order

- Nested-loop join: compare each outer tuple with inner tuples. Simple; good when the outer input is tiny or the inner side has an index.
- Block nested-loop: load a block of outer pages and scan the inner relation once per block; reduces repeated scans.
- Sort-merge join: sort both inputs by join key (unless already ordered), then scan together; useful for ordered output or large inputs.
- Hash join: hash one relation into buckets, then probe with the other; often efficient for equality joins when memory is adequate.

If R has 100 pages and S has 1,000 pages, a naive tuple nested loop can be expensive. A block nested-loop with M buffer pages can scan S about ceil(B(R)/(M-2)) times, while a hash join is roughly linear in total pages under favorable memory/distribution assumptions. Cost formulas depend on buffer availability and whether inputs are already sorted or indexed.

### Query-plan interpretation

A plan tree often has table scans/index scans at leaves, then selection, join, grouping, and sort operators. A selective predicate can make an index useful; if most rows qualify, a sequential scan may be cheaper. The optimizer estimates cardinality from statistics, so stale or skew-blind statistics can lead to a poor plan.

### Practice questions

1. Why do B+ tree leaves link to one another? Efficient ordered/range scans.
2. When is a table scan reasonable despite an index? When many rows qualify or random index lookups cost more.
3. What join is naturally suited to equality keys and enough memory? Hash join.
4. Which index helps range queries with ordered key values? B+ tree.
