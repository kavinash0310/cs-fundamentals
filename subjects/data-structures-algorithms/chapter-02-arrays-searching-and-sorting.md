# Chapter 2 — Arrays, Searching, and Sorting

## Learning goals

By the end, you should be able to reason about array access and insertion, search correctly in sorted data, trace the main comparison sorts, compare stability and memory costs, and select an algorithm from the input constraints.

## 1. Array model and operations

An array stores elements in contiguous indexed slots. If the base address is B and each element occupies w bytes, the address of A[i] is B + i·w (for zero-based indexing). That calculation makes indexed access Θ(1).

~~~text
index:    0       1       2       3       4
        +-------+-------+-------+-------+-------+
A       |  12   |  18   |  25   |  31   |  44   |
        +-------+-------+-------+-------+-------+
address: B     B+w    B+2w    B+3w    B+4w
~~~

Typical costs for an unsorted array of n elements:

| Operation | Cost | Reason |
|---|---:|---|
| Read/write A[i] | Θ(1) | Address computed from index |
| Search by value | Θ(n) | May inspect every element |
| Insert at end with free capacity | Θ(1) | Write next slot |
| Insert at beginning or middle | Θ(n) | Shift a suffix |
| Delete at beginning or middle | Θ(n) | Close the gap by shifting |
| Traverse all values | Θ(n) | Visit each element |

A dynamic array stores a length and capacity. Its logical length can grow while physical capacity remains fixed until reallocation is needed. A resize typically allocates a larger block and copies values; geometric growth gives amortized constant append cost, as shown in Chapter 1.

## 2. Strings and array patterns

A string is often represented as a sequence of characters; in C, a string also needs a terminating null character. Common array problems are easier when the loop invariant is explicit.

### Two pointers

For a sorted array, two pointers can move inward from the ends. To find whether a pair sums to target:

~~~text
left = 0; right = n-1
while left < right:
    s = A[left] + A[right]
    if s == target: report pair
    else if s < target: left = left + 1
    else: right = right - 1
~~~

If s is too small, pairing A[left] with any index at or before right cannot reach a larger sum because the array is sorted; discarding left is safe. Each move changes a pointer, so time is Θ(n), extra space Θ(1). If the array is not sorted, this reasoning fails.

### Prefix sums

Define P[0]=0 and P[k+1]=P[k]+A[k]. Then the sum of A[l..r] is P[r+1]-P[l]. Building P takes Θ(n) time and space; each inclusive range-sum query then takes Θ(1). This is useful for many static range queries. If updates are frequent, another structure may be more suitable.

## 3. Linear search

~~~text
for i = 0 to n-1:
    if A[i] == x: return i
return NOT_FOUND
~~~

Worst-case comparisons are n; best case is 1; with uniform successful position, expected comparisons are (n+1)/2. The algorithm requires no sortedness and uses Θ(1) extra space.

Be precise about duplicate values: this version returns the first occurrence. A version scanning backward may return the last occurrence. Interview questions often care about that detail.

## 4. Binary search and boundary discipline

Binary search requires a sorted array and repeatedly eliminates half of a candidate interval. Use a half-open interval [lo, hi): lo is included and hi is excluded.

~~~text
lo = 0; hi = n
while lo < hi:
    mid = lo + (hi-lo)/2
    if A[mid] < x:
        lo = mid + 1
    else:
        hi = mid
if lo < n and A[lo] == x: return lo
return NOT_FOUND
~~~

This version returns the lower bound: the first position whose value is at least x. It is also the insertion position that keeps the array sorted. On termination, every index below lo has a value < x and every index at or above hi has a value ≥ x; when lo=hi, the boundary is determined. If x occurs, this boundary is its first occurrence.

The interval shrinks by at least half each iteration, giving Θ(log n) time and Θ(1) extra space. For an empty array, lo=hi=0 and the final bounds check prevents indexing it.

**Trace:** A = [2, 4, 4, 4, 9], x=4.

| Step | lo | hi | mid | A[mid] | Action |
|---|---:|---:|---:|---:|---|
| 1 | 0 | 5 | 2 | 4 | hi = 2 |
| 2 | 0 | 2 | 1 | 4 | hi = 1 |
| 3 | 0 | 1 | 0 | 2 | lo = 1 |
| End | 1 | 1 | — | — | A[1] is first 4 |

### Binary-search pitfalls

- Mixing inclusive and half-open interval conventions.
- Updating lo = mid rather than mid+1 when A[mid] < x; this may not shrink the interval.
- Computing (lo+hi)/2 in a fixed-width integer type when the sum may overflow; lo+(hi-lo)/2 avoids that sum.
- Forgetting duplicates, empty input, or values outside the range.
- Applying binary search to data that is not ordered by the searched key.

## 5. What makes a sorting algorithm useful?

A sorting algorithm rearranges records by a key. Several properties matter:

- **Stable:** records with equal keys preserve their original relative order.
- **In-place:** uses little auxiliary memory; exact convention varies.
- **Adaptive:** benefits when input is already or nearly sorted.
- **Comparison sort:** determines order through pairwise comparisons.

For comparison-based sorting, any algorithm that must distinguish all n! possible input orders has worst-case comparison count Ω(log₂(n!)) = Ω(n log n). This is the comparison-sorting lower bound. It does not apply to non-comparison techniques that exploit restricted keys, such as counting sort.

## 6. Insertion sort

Maintain a sorted prefix. Take the next key and shift larger prefix elements right until the insertion position is open.

~~~text
for i = 1 to n-1:
    key = A[i]
    j = i-1
    while j >= 0 and A[j] > key:
        A[j+1] = A[j]
        j = j-1
    A[j+1] = key
~~~

**Trace on [5, 2, 4, 1]:**

~~~text
start:       [5 | 2, 4, 1]
insert 2:    [2, 5 | 4, 1]
insert 4:    [2, 4, 5 | 1]
insert 1:    [1, 2, 4, 5]
~~~

The strict comparison A[j] > key preserves equal-key order, so this form is stable. It is in-place. Best case for already sorted input is Θ(n); average and worst case are Θ(n²). It works well for small or nearly sorted arrays and is often used inside hybrid library sorts.

## 7. Selection sort

At position i, find the minimum in A[i..n-1], then swap it into position i. It makes Θ(n²) comparisons regardless of input order and at most n-1 swaps. Basic selection sort is in-place but generally unstable: swapping a distant minimum can change the order of equal records. Its small number of writes can matter when writes are expensive.

## 8. Bubble sort

Repeatedly compare adjacent out-of-order pairs and swap them; the largest unsorted value moves to the end each pass. With a “swapped” flag, already sorted input can finish in Θ(n), while the average and worst cases remain Θ(n²). The adjacent-swap form is stable when it swaps only on strict greater-than. Bubble sort is mainly pedagogical; use a well-tested library sort in production.

## 9. Merge sort

Split the array, sort both halves recursively, then merge two sorted sequences by repeatedly taking the smaller front item.

~~~text
[8, 3, 6, 2]
       split
[8, 3]           [6, 2]
[3, 8]           [2, 6]
       merge
[2, 3, 6, 8]
~~~

The recurrence T(n)=2T(n/2)+Θ(n) yields Θ(n log n) in best, average, and worst cases. Standard array merge sort needs Θ(n) extra array space and Θ(log n) call-stack depth. Choosing from the left half on equal keys makes it stable. It guarantees n log n time and is useful when stability matters or when sorting linked data/external files.

## 10. Quicksort

Choose a pivot, partition elements around it, then recursively sort the two sides. Partitioning is Θ(n). With balanced partitions, T(n)=2T(n/2)+Θ(n)=Θ(n log n). With repeatedly extreme partitions, T(n)=T(n-1)+Θ(n)=Θ(n²). Randomized pivot selection gives expected Θ(n log n) for distinct keys under the usual model, but worst-case remains Θ(n²) unless the implementation includes safeguards. Typical in-place implementations are not stable.

**Partition reasoning:** after partition, every item in the left region is no greater than the pivot and every item in the right region is no less (depending on the chosen scheme). The pivot is in a position where its final ordering is established. Be careful: different partition schemes have different return-index conventions.

## 11. Heap sort

Build a max-heap, repeatedly move the maximum at the root to the end, shrink the heap, and restore the heap property. Bottom-up heap construction is Θ(n), then n extractions cost Θ(n log n); total Θ(n log n), O(1) auxiliary array space, and usually not stable. Chapter 5 explains heaps in detail.

## 12. Comparison table

| Algorithm | Best | Average | Worst | Extra space | Stable? |
|---|---:|---:|---:|---:|---|
| Insertion | Θ(n) | Θ(n²) | Θ(n²) | Θ(1) | Yes |
| Selection | Θ(n²) | Θ(n²) | Θ(n²) | Θ(1) | Usually no |
| Bubble with early exit | Θ(n) | Θ(n²) | Θ(n²) | Θ(1) | Yes |
| Merge | Θ(n log n) | Θ(n log n) | Θ(n log n) | Θ(n) | Yes |
| Quicksort | Θ(n log n) expected | Θ(n log n) expected | Θ(n²) | Θ(log n) expected stack | Usually no |
| Heapsort | Θ(n log n) | Θ(n log n) | Θ(n log n) | Θ(1) | No |

Exact bounds can depend on variant, duplicates, pivot policy, and implementation. State which version is assumed.

## 13. Choosing under constraints

- Small or nearly sorted input: insertion sort may be simple and fast.
- Need stable guaranteed O(n log n): merge sort.
- Need in-place guaranteed O(n log n): heapsort.
- Need average-fast in-memory sorting: a robust quicksort or introsort implementation.
- Integer keys in a narrow range: counting sort can be Θ(n+k), where k is key-range size, but its assumptions and memory cost matter.
- Need first/last matching duplicate: use lower/upper bound rather than an arbitrary binary-search hit.

In interviews, first clarify ordering, duplicates, mutation permission, stability, memory constraints, and expected input size.

## 14. Common questions

**Q1.** Why does binary search take logarithmic time?  
**Answer:** Each comparison reduces the remaining candidate interval by a constant factor, roughly half; after k steps, size is about n/2ᵏ.

**Q2.** Is binary search O(1) space?  
**Answer:** Iterative binary search is O(1) auxiliary space. A recursive implementation uses O(log n) call-stack space.

**Q3.** Why can comparison sorting not generally beat n log n?  
**Answer:** A comparison decision tree needs at least n! leaves for all permutations; its height is at least log₂(n!) = Ω(n log n).

**Q4.** Why is insertion sort adaptive?  
**Answer:** The number of shifts depends on existing inversions; a nearly sorted array requires few shifts.

**Q5.** Is quicksort always O(n log n)?  
**Answer:** No. That is expected under common randomized assumptions and balanced worst case; its worst case is Θ(n²).

## 15. Practice

1. Run lower-bound binary search for x=6 in [1, 3, 6, 6, 8, 10].
2. Trace insertion sort on [7, 3, 5, 3] and show each sorted prefix.
3. For n=8, identify the best and worst comparison growth of insertion sort.
4. Explain why ordinary merge sort needs extra memory for arrays but can merge linked lists differently.
5. Give an input arrangement that causes a first-element-pivot quicksort to make maximally unbalanced splits.
6. A key range is 0 through 99 and n is 10 million. When might counting sort be preferable? What extra memory is required?
7. Distinguish lower bound, lower-bound position, and binary-search match.

**Solutions:** 1) Lower bound is index 2. 2) [3,7,5,3] → [3,5,7,3] → [3,3,5,7]. 3) Best Θ(n), worst Θ(n²). 4) Array merging needs a destination or careful additional storage; linked lists can relink nodes using pointers, though implementation details and stability still matter. 5) Already sorted input, if the pivot is always the first item and partitioning places it at an endpoint. 6) Counting sort may be attractive because k=100 is small relative to n; it uses Θ(k) counts and often Θ(n) output storage for a stable version. 7) A lower bound is an asymptotic complexity concept; a lower-bound search position is first index with value ≥ x; an arbitrary match is any equal element.

## Notebook work

Draw one full insertion-sort pass and a merge of two sorted lists. Then reproduce the lower-bound binary-search trace without looking. For every algorithm, write its precondition and one edge case.

**Next:** Chapter 3 covers linked structures, stacks, queues, and deques.
