# Chapter 5 — Binary Heaps, Priority Queues, and Hashing

## Learning goals

Represent a complete binary tree in an array, implement heap operations, explain why bottom-up heap construction is linear, and reason about hash tables, collisions, load factor, and expected versus worst-case cost.

## 1. Binary heap

A binary heap is a complete binary tree that satisfies an ordering property. In a max-heap, each parent key is at least as large as its children; the root is maximum. In a min-heap, each parent is at most its children; the root is minimum. A heap is not fully sorted: siblings and unrelated subtrees need not be ordered relative to each other.

~~~text
             90
           /    \
         70      65
        / \     /
       20  45  50

array: [90, 70, 65, 20, 45, 50]
~~~

For zero-based array index i:

- Parent: floor((i-1)/2), for i > 0.
- Left child: 2i+1.
- Right child: 2i+2.

Because the tree is complete, it has height Θ(log n), and these relationships need no explicit child pointers.

## 2. Heap operations

### Peek

The maximum in a max-heap is at the root, so peek is Θ(1).

### Insert

Append the new value at the end to preserve completeness. While it is greater than its parent, swap upward (sift-up). At most the height many swaps occur, so insertion is O(log n).

Insert 80 into [90,70,65,20,45,50]:

~~~text
append: [90,70,65,20,45,50,80]
80 > parent 65, swap:
        [90,70,80,20,45,50,65]
80 < parent 90, stop
~~~

### Extract maximum

Save the root, move the last item to the root, remove the last slot, then sift down by swapping with the larger child while the parent is smaller. Cost O(log n).

### Heapify

Sift-down repairs a node whose child subtrees are already heaps. Build a heap bottom-up by applying sift-down from the last internal node back to the root.

Although one sift-down can cost O(log n), heap construction is O(n): most nodes lie near the leaves and move only a few levels. A height-based sum is bounded by n(1/2 + 2/4 + 3/8 + ...), which is O(n).

## 3. Heapsort and priority queues

Heapsort builds a max-heap, swaps the maximum with the last heap element, shrinks the heap, and sifts down. It takes Θ(n log n) time in all cases, uses O(1) auxiliary array space in the standard form, and is not stable.

A priority queue supports insert, peek highest/lowest priority, and extract highest/lowest priority. A heap provides O(log n) insertion and extraction, O(1) peek, and O(n) search for an arbitrary value. If priorities tie, add a stable sequence number when deterministic arrival order is required.

Use a heap when repeatedly selecting the next extreme item; use a sorted array/list if the workload has different update/query trade-offs.

## 4. Hash tables

A hash table maps a key k to an array index using a hash function h(k). Ideally, keys are spread uniformly. Since the table has finite capacity, distinct keys can map to the same slot: this is a collision, and every design must handle it.

The load factor is α = n/m, where n is the number of stored keys and m is table capacity. Under uniform hashing and a controlled load factor, search/insert/delete are expected O(1), but worst case is Θ(n). Expected time depends on the hashing assumptions and policy; it is not a universal worst-case guarantee.

## 5. Separate chaining

Each table slot points to a bucket, often a linked list or dynamic collection. Colliding keys share that bucket.

~~~text
slot 0 -> (key A) -> (key F) -> NULL
slot 1 -> (key C) -> NULL
slot 2 -> NULL
slot 3 -> (key B) -> (key D) -> NULL
~~~

Expected chain length is about α under uniform hashing. Search checks the selected chain; worst case all n keys land in one bucket. Deletion is straightforward once the node is found.

## 6. Open addressing

All keys live directly in the table. On collision, probe a sequence of alternative slots.

- **Linear probing:** h(k,i)=(h(k)+i) mod m. Simple and cache-friendly, but clusters can build up.
- **Quadratic probing:** probe offsets based on i². Reduces primary clustering but may fail to visit every slot depending on parameters.
- **Double hashing:** use a second hash for the step size. It tends to spread probe sequences more widely; choose parameters so the step is relatively prime to m.

Insertion checks slots along the probe sequence. Search follows the same sequence and stops at a never-used empty slot; a deleted slot must be marked as a tombstone so a later key in the sequence is still findable. Inserting into a tombstone is allowed, but searches cannot treat it as never-used.

**Linear-probing trace:** Let m=7 and h(k)=k mod 7. Insert 10, 17, 24. All hash to slot 3. They occupy slots 3, 4, and 5 in order. Search for 24 follows 3, 4, 5. This illustrates primary clustering.

Open addressing requires capacity to remain available; as α approaches 1, probe lengths rise sharply. Resizing and rehashing are common.

## 7. Hash functions and resizing

A hash function must be deterministic for the same key during a table's lifetime, and should distribute keys well. For strings, a polynomial rolling hash can combine character codes; careless arithmetic overflow or poor modulus choices can cause clustering. A cryptographic hash is designed for adversarial security and is usually more expensive than a table's ordinary hash.

When a table grows, allocate a larger array and reinsert every key using the new modulus/capacity. Old bucket indices cannot simply be copied because h(k) may change. Geometric capacity growth makes total rehash work over n inserts amortized O(n), though a single resize costs Θ(n).

## 8. Complexity summary

| Structure / operation | Typical time | Worst case |
|---|---:|---:|
| Heap peek extreme | Θ(1) | Θ(1) |
| Heap insert/extract | O(log n) | O(log n) |
| Heap build | Θ(n) | Θ(n) |
| Heap arbitrary search | O(n) | O(n) |
| Hash lookup/insert/delete | Expected O(1) under assumptions | Θ(n) |
| Rehash resize | Amortized O(1) per insertion | Θ(n) for one resize |

## 9. Frequent mistakes

- Treating a heap array as sorted.
- Using the one-based child formulas for a zero-based array.
- Claiming heap construction is O(n log n) because each sift-down can be logarithmic; the bottom-up sum is O(n).
- Claiming a hash table guarantees O(1) in the worst case.
- Forgetting tombstones in open-addressed deletion.
- Rehashing by copying old slots without recomputing indices.
- Ignoring load factor, collision policy, or adversarial keys.

## 10. Questions and answers

**Q1.** What is the largest element in a max-heap? **Answer:** The root.
**Q2.** Is every parent smaller than every node in the right subtree? **Answer:** No. Heap order is only parent-child order.
**Q3.** Why is heap insertion O(log n)? **Answer:** The new item can travel up at most the tree height.
**Q4.** Why can average hash search be constant but worst-case linear? **Answer:** Uniform distribution keeps expected buckets short; collisions can still place all keys together.
**Q5.** Why can't a tombstone terminate an open-addressing search? **Answer:** A matching key may have been placed further along the same probe sequence.

## 11. Practice with answers

1. For n=10, give children of zero-based index 3. **Answer:** 7 and 8 (if within the heap).
2. Insert 95 into max-heap [90,70,80,20,45,50,65]. **Answer:** append 95, swap upward with 20, then 70, then 90: [95,90,80,70,45,50,65,20].
3. Which heap operation is O(1)? **Answer:** Peek at the root.
4. Table size 11, h(k)=k mod 11. Insert 21,32,43 by linear probing. **Answer:** Slots 10,0,1.
5. What is α for 18 keys in 24 slots? **Answer:** 0.75.
6. Why is bottom-up build-heap linear? **Answer:** Many nodes are near leaves and can move only a small distance; summing height-weighted nodes is O(n).
7. State a worst-case hash-table lookup bound. **Answer:** Θ(n).

## Notebook work

Draw sift-up and sift-down paths, annotate zero-based parent/child indices, and show a collision chain plus a linear-probing table with a tombstone.

**Next:** Chapter 6 introduces graph representations, BFS, DFS, and topological ordering.
