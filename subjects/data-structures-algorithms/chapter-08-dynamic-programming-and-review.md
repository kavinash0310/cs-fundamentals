# Chapter 8 — Dynamic Programming and Course Review

## Learning goals

Identify overlapping subproblems and optimal substructure, define states and transitions precisely, implement memoized and tabulated solutions, reconstruct an answer, and distinguish dynamic programming from greedy choice.

## 1. When dynamic programming fits

Dynamic programming (DP) is useful when:

1. The problem has **optimal substructure**: an optimal answer can be composed from optimal answers to smaller states.
2. The computation has **overlapping subproblems**: the same smaller state would otherwise be solved repeatedly.

A DP state must include enough information to determine future choices. A recurrence is only meaningful after the state, base cases, and transition have been stated.

### A repeatable recipe

1. Define the state in words and symbols.
2. Identify decisions that lead to smaller states.
3. Write a recurrence over those decisions.
4. Set base cases.
5. Choose a valid evaluation order (or memoize recursively).
6. Identify the requested final state.
7. Store choices or recompute transitions if an actual solution must be reconstructed.
8. Analyze number of states × work per state, plus memory.

## 2. Fibonacci: a first example

The direct recursive definition F(n)=F(n-1)+F(n-2) repeats the same calls many times and takes exponential time. Memoization stores each result once:

~~~text
fib(n):
    if n <= 1: return n
    if memo[n] exists: return memo[n]
    memo[n] = fib(n-1) + fib(n-2)
    return memo[n]
~~~

There are n+1 states and O(1) work per state, so O(n) time and O(n) memory. Bottom-up tabulation fills F[0],F[1],...,F[n]; if only the value is required, rolling two variables reduces auxiliary memory to O(1).

This example illustrates memoization, but Fibonacci alone is not enough to understand DP: the harder part is designing a sufficient state.

## 3. 0/1 knapsack

There are n items; item i has positive weight wᵢ and value vᵢ. Each item may be taken at most once. The total weight cannot exceed capacity W.

Define DP[i][c] as the maximum value using the first i items with capacity c.

~~~text
DP[0][c] = 0
DP[i][c] = DP[i-1][c]                         if w_i > c
DP[i][c] = max(DP[i-1][c],
               v_i + DP[i-1][c-w_i])          otherwise
answer = DP[n][W]
~~~

The two choices are skip item i or take it once. The previous row ensures it cannot be taken repeatedly. There are O(nW) states, O(1) work per state, so time and full-table memory are O(nW). This is pseudo-polynomial: W is a numeric value, not its number of input bits.

Example: capacity 5; items (weight,value) are (2,3),(3,4),(4,6). Best is items 1 and 2, total weight 5 and value 7. A greedy rule based only on highest value or value/weight is not generally correct for 0/1 knapsack.

A one-dimensional table can reduce memory to O(W), but capacities must be iterated downward:

~~~text
for each item (w,v):
    for c = W down to w:
        dp[c] = max(dp[c], v + dp[c-w])
~~~

Descending order prevents this item's updated value from being reused in the same iteration. For unbounded knapsack, where items may be reused, ascending order is appropriate. This direction is a frequent exam trap.

## 4. Longest common subsequence (LCS)

A subsequence preserves order but may skip elements; it need not be contiguous. Let X have length m and Y length n. Define L[i][j] as the LCS length of prefixes X[0..i-1] and Y[0..j-1].

~~~text
L[0][j] = 0; L[i][0] = 0
if X[i-1] == Y[j-1]:
    L[i][j] = 1 + L[i-1][j-1]
else:
    L[i][j] = max(L[i-1][j], L[i][j-1])
~~~

The table has Θ(mn) states and O(1) work each: Θ(mn) time and space. To reconstruct a subsequence, start at L[m][n]; when characters match, record it and move diagonally; otherwise move toward the neighboring cell with the larger value.

**Example:** X=ABCBDAB and Y=BDCABA. The LCS length is 4; one LCS is BCBA (others may exist).

Do not confuse subsequence with substring. The longest common substring must be contiguous and has a different recurrence.

## 5. Coin change

Given coin denominations and target amount A, the minimum-coin problem asks for the fewest coins whose values sum to A, with unlimited reuse unless specified.

Let dp[x] be the minimum coins needed for amount x. Set dp[0]=0 and all positive amounts to infinity initially. For each amount x, try every coin c ≤ x:

~~~text
dp[x] = min over coins c<=x of (1 + dp[x-c])
~~~

The base case and unreachable amounts matter. With C denominations, a bottom-up amount-first implementation takes O(AC) time and O(A) space. Greedy largest coin first is not always optimal: for denominations {1,3,4} and amount 6, greedy gives 4+1+1 (3 coins), while 3+3 uses 2.

If each coin can be used once, the state and iteration order must change; do not silently reuse coins.

## 6. Matrix-chain multiplication

Given matrices A₁...Aₙ with compatible dimensions, multiplication order affects the number of scalar multiplications even though the final product is the same. Let m[i][j] be the minimum cost of multiplying Aᵢ through Aⱼ. For every split k:

~~~text
m[i][i] = 0
m[i][j] = min over i<=k<j of
          (m[i][k] + m[k+1][j] + p[i-1]*p[k]*p[j])
~~~

There are Θ(n²) intervals and Θ(n) possible splits each, giving Θ(n³) time and Θ(n²) space. Fill shorter intervals before longer intervals. The algorithm chooses parenthesization; it does not change matrix values or the order of the original factors.

## 7. Memoization versus tabulation

| Memoization | Tabulation |
|---|---|
| Top-down recursion with cache | Bottom-up table |
| Computes only states reached | May compute every table state |
| Call-stack overhead | Explicit iteration order |
| Natural from a recurrence | Often easier to control memory |

Both rely on the same state graph. A dependency arrow points from a state to the smaller states it needs. A valid tabulation order processes dependencies first.

## 8. DP versus greedy

A greedy algorithm commits to a locally best choice; a DP compares alternatives across states. Greedy can be faster and use less memory, but only if a correctness proof supports the choice.

For interval activity selection, earliest finish time is provably safe. For 0/1 knapsack, choosing the item with the highest value/weight ratio can fail. Before using greedy, prove an exchange or cut property; otherwise formulate states that preserve all relevant choices.

## 9. Complexity and state design

A DP can be too slow or too large if the state space is poorly chosen. For each solution, count:

- Number of distinct states.
- Work per state (including loops over transitions).
- Storage for table, decisions, and recursion stack.
- Numeric ranges and whether the complexity is polynomial in input encoding length.

For 0/1 knapsack, O(nW) is pseudo-polynomial and can be exponential in the number of bits used to encode W. That distinction matters in theoretical analysis.

## 10. Mixed solved review

### Problem A: trace binary search

Sorted A=[2,4,4,4,9], find lower bound of 4 using [lo,hi)=[0,5). Midpoints visited are 2, then 1, then 0. The interval ends at [1,1), so first 4 is at index 1.

### Problem B: heap build

Array [4,10,3,5,1], build a max-heap bottom-up. Sift the last internal node at index 1: 10 already dominates children 5 and 1. Sift index 0: swap 4 with 10, then compare 4 with 5 and 1 and swap with 5. Result [10,5,3,4,1].

### Problem C: BFS distance

Edges S-A, S-B, A-C, B-C, C-D. BFS from S sets distances d(S)=0, d(A)=d(B)=1, d(C)=2, d(D)=3. Either A or B may become C's parent depending on neighbor order; distance remains 2.

### Problem D: sorting choice

If records must remain in original order for equal keys and worst-case O(n log n) is required, stable merge sort is suitable; account for Θ(n) extra array memory. Heapsort is in-place but not stable.

### Problem E: identify an invalid claim

“Dijkstra works because every visited vertex has the smallest tentative distance, even if some edge weights are negative.” Invalid: a later negative edge can make an already-finalized distance smaller. Non-negative weights are required.

## 11. Exam and interview practice

For each problem, first state assumptions and the invariant; then explain the algorithm, prove correctness briefly, and analyze time and auxiliary space.

1. A sorted array contains duplicates. Design a search returning the first occurrence. Give an invariant and complexity.
2. A stream needs its largest k values seen so far. Which structure is useful? State its size and per-item cost.
3. A sparse unweighted graph needs the fewest-edge route from s. Select representation and algorithm.
4. A directed dependency graph may contain cycles. How can you detect whether all tasks can be ordered?
5. A weighted undirected graph needs minimum-cost connectivity. Select the algorithm and cycle-check structure.
6. A graph may contain negative edges. Which single-source algorithm can identify reachable negative cycles?
7. A problem asks whether a list is a palindrome. Give an O(n)-time approach and discuss O(1) auxiliary-space trade-offs.
8. For a dynamic array with doubling, explain amortized append without claiming each append costs O(1).
9. For 0/1 knapsack, explain why capacities iterate downward in the one-dimensional implementation.
10. Give one case where a hash table's worst-case lookup is Θ(n).

**Answer guide:** 1) Lower bound with half-open interval; Θ(log n), O(1) iterative space. 2) Min-heap of size k; O(log k) update after initialization, O(k) storage. 3) Adjacency list plus BFS, Θ(V+E). 4) Kahn's algorithm; fewer than V outputs means a cycle; Θ(V+E). 5) Kruskal plus DSU, O(E log E). 6) Bellman-Ford, O(VE). 7) Reverse second half or use a stack; restoration of a mutated list may be needed; O(1) auxiliary-space pointer approach. 8) Sum geometric copies across the sequence. 9) Descending order prevents using an item multiple times in one iteration. 10) All keys collide into one chain/probe cluster.

## 12. Rapid revision map

| Topic | Key invariant or guarantee |
|---|---|
| Binary search | Candidate interval shrinks; requires sorted order |
| Linked list | Links and head/tail must remain consistent |
| Stack | LIFO |
| Queue | FIFO |
| BST | Left keys < node < right keys under policy |
| Heap | Parent-child order plus completeness |
| Hash table | Expected fast access depends on hash/load assumptions |
| BFS | Minimum number of edges from source |
| DFS | Reachability and structural exploration |
| Kruskal/Prim | Minimum spanning tree |
| Dijkstra | Non-negative edge weights |
| Bellman-Ford | Negative edges and reachable negative-cycle detection |
| Dynamic programming | State, transition, base, order |

## Notebook work

From memory, write the DP recipe and solve one 0/1 knapsack table cell-by-cell. Then teach aloud the distinction between MST, shortest-path tree, BFS, and Dijkstra. Finish by writing one correctness argument and one time/space analysis for an algorithm you choose.

## Course completion checklist

You should now be able to:

- Analyze time, auxiliary space, and common recurrences.
- Trace array searching/sorting and explain stability and worst cases.
- Implement lists, stacks, queues, trees, heaps, and hash-table collision policies.
- Select matrix/list graph representations and trace BFS/DFS.
- Prove greedy choices or use DP when local choice lacks proof.
- Solve core GATE-style algorithm questions and explain design trade-offs in interviews.
- Teach each structure with a labelled diagram and a short example.

This eight-chapter course covers the standard DSA core. It does not replace the other GATE CS areas or the exact subject/pedagogy content in a future Bihar teacher recruitment notice.
