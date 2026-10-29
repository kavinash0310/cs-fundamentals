# Chapter 1 — Algorithm Analysis and Correctness

## Why this chapter matters

A data structure is useful only when its operations fit the problem. Algorithm analysis lets us compare solutions independently of a particular laptop, compiler, or programming language. Correctness reasoning explains why the chosen procedure returns the right result for every permitted input.

After this chapter, you should be able to count primitive work, compare growth rates, solve common recurrences, distinguish auxiliary space from input storage, and prove a loop or recursive algorithm correct.

## 1. Start with a precise problem

Before counting steps, state what is allowed.

**Problem:** Given an array A of n integers and a target x, return an index i such that A[i] = x, or report that x is absent.

Questions that change the algorithm include: Is A sorted? Can values repeat? Must we return the first matching index? Is modifying A allowed? The same task may need linear search, binary search, or a hash table depending on these conditions.

We measure input size using a parameter such as n (number of elements), m (another collection size), or V and E (vertices and edges). For a graph algorithm, V alone may hide important work: reading an adjacency list costs proportional to V + E.

## 2. Count work before simplifying it

Consider:

~~~text
sum = 0
for i = 0 to n-1:
    sum = sum + A[i]
~~~

The loop body runs n times, so T(n) = c₀ + c₁n for fixed costs c₀ and c₁. For large n, the linear term dominates; we write T(n) = Θ(n).

For nested loops, do not automatically multiply the bounds. The inner range may depend on the outer index.

~~~text
count = 0
for i = 1 to n:
    for j = 1 to i:
        count = count + 1
~~~

The total is 1 + 2 + ... + n = n(n+1)/2 = Θ(n²). A loop that repeatedly halves a quantity has only about log₂ n iterations because after k halvings it reaches n/2ᵏ; solve n/2ᵏ ≤ 1 to get k ≥ log₂ n.

### A counting checklist

1. Name every input-size parameter.
2. Identify the basic operation (comparison, array access, arithmetic operation, or edge inspection).
3. Count how many times it executes.
4. Simplify the expression using asymptotic notation.
5. State the case and assumptions (best, average, or worst; sortedness; data distribution).

## 3. O, Ω, and Θ

For non-negative functions f and g, f(n) = O(g(n)) means that beyond some n₀, f(n) is at most a constant multiple of g(n). Formally, there are constants c > 0 and n₀ such that 0 ≤ f(n) ≤ c g(n) for all n ≥ n₀. Big O is an asymptotic upper bound; it is not automatically the tight bound.

Similarly, f(n) = Ω(g(n)) is an asymptotic lower bound, and f(n) = Θ(g(n)) means both bounds hold. If an algorithm has T(n) = 3n² + 8n + 4, then T(n) is Θ(n²), O(n²), and Ω(n²). It is also O(n³), but that description is less informative.

A common misconception: “Big O means worst case.” Big O describes a bound. We often analyze worst-case running time and express that function with O or Θ, but best-case running time can also be O(n).

### Growth rates to remember

From slower to faster growth:

~~~text
1 < log n < n < n log n < n² < n³ < cⁿ < n!
~~~

For fixed bases greater than 1, logarithms differ only by a constant factor: log₂ n = log₁₀ n · log₂ 10. Thus log bases are interchangeable in Θ notation.

**Worked comparison:** For n = 1,000,000, n is about one million, n log₂ n is about twenty million, and n² is about one trillion. The constants matter for small inputs, but an extra polynomial factor eventually dominates any fixed constant.

## 4. Best, average, and worst case

For linear search on an unsorted array:

- Best case: the first element matches, so Θ(1).
- Worst case: the target is last or absent, so Θ(n).
- Average case depends on a probability model. If the target is equally likely to be at any of n positions, expected comparisons are (n+1)/2, which is Θ(n).

Always state the assumed distribution when giving an average case. In exam settings, a question may specify uniform distribution or ask only for worst case.

## 5. Space complexity

**Input space** stores the original data. **Auxiliary space** is extra memory used by the algorithm, often excluding the input and sometimes excluding output; follow the question's convention.

An iterative sum uses Θ(1) auxiliary space. A recursive procedure may use Θ(depth) call-stack space even if each call has only a few local variables. Merge sort uses Θ(n) auxiliary array space in a typical implementation. In-place sorting usually means the algorithm uses only a small amount of extra storage, often O(1) or O(log n); definitions can vary for recursion stacks.

When analyzing space, count simultaneously live memory, not the total number of allocations over the whole execution.

## 6. Amortized cost: expensive operations can be rare

Suppose a dynamic array doubles capacity when full. A single append that triggers resizing costs Θ(n), because n items must be copied. Yet appending n items from an empty array takes only Θ(n) total copying plus Θ(n) ordinary writes.

If capacities double, copied elements across expansions are 1 + 2 + 4 + ... + 2ᵏ < 2n. Therefore total work for n appends is O(n), so the amortized cost per append is O(1). This is a guarantee over a sequence of operations; it does not claim every individual append is constant time.

## 7. Recurrences for recursive algorithms

A recurrence describes work on smaller instances. For merge sort:

~~~text
T(n) = 2T(n/2) + cn
T(1) = d
~~~

At each level, merging across all subproblems costs cn. There are log₂ n levels, so T(n) = Θ(n log n).

### Recursion-tree example

For T(n) = 2T(n/2) + n:

~~~text
level 0:              n
level 1:          n/2 + n/2 = n
level 2:      n/4 + n/4 + n/4 + n/4 = n
...
last level:                         n
number of levels: Θ(log n)
total: Θ(n log n)
~~~

### Master theorem (standard form)

For T(n) = aT(n/b) + f(n), with a ≥ 1 and b > 1, compare f(n) with n^(log_b a):

1. If f(n) is polynomially smaller, then T(n) = Θ(n^(log_b a)).
2. If f(n) = Θ(n^(log_b a) logᵏ n), then T(n) = Θ(n^(log_b a) log^(k+1) n).
3. If f(n) is polynomially larger and the regularity condition holds, then T(n) = Θ(f(n)).

Example: T(n) = 3T(n/2) + n. Here n^(log₂3) ≈ n^1.585 dominates n, so T(n) = Θ(n^(log₂3)). The theorem does not apply mechanically to recurrences with unequal subproblem sizes, non-constant a or b, or forms outside its conditions; use substitution or a recursion tree instead.

## 8. Correctness with loop invariants

A loop invariant is a statement that is true before and after each iteration. To prove an algorithm using one:

1. **Initialization:** show it holds before the first iteration.
2. **Maintenance:** assuming it holds at the start of an iteration, prove it still holds after the body.
3. **Termination:** combine the invariant with the loop's stopping condition to establish the required result.

### Example: maximum of a non-empty array

~~~text
best = A[0]
for i = 1 to n-1:
    if A[i] > best:
        best = A[i]
return best
~~~

Invariant before iteration i: best is the maximum of A[0..i-1]. Initially, for i = 1, the prefix contains only A[0], so the claim holds. Each iteration compares A[i] with the prefix maximum, so best becomes the maximum of A[0..i]. When the loop ends after processing index n-1, best is the maximum of the entire array. The precondition n ≥ 1 is essential.

### Binary-search invariant preview

For a half-open search interval [lo, hi), a useful invariant is: if target exists in the array, one valid occurrence lies in A[lo..hi). Each comparison discards a region that cannot contain the target. The interval shrinks until it is empty or a match is found.

## 9. Worked analysis problems

**Problem A:** What is the complexity of the following?

~~~text
i = 1
while i < n:
    for j = 1 to n:
        work()
    i = 2*i
~~~

The outer loop runs Θ(log n) times; the inner loop runs n times per outer iteration. Total Θ(n log n).

**Problem B:** Analyze a triangular loop where j runs from 0 to i for every i from 0 through n-1. Its work is Σ(i+1) = n(n+1)/2 = Θ(n²).

**Problem C:** Analyze a recursive procedure that calls itself once on n-1 and performs constant work. T(n)=T(n-1)+Θ(1)=Θ(n), with Θ(n) stack space. The recurrence has n levels.

## 10. Common errors

- Dropping the input-size parameter or counting only the outer loop.
- Calling an upper bound “the exact complexity.”
- Saying average-case without a probability model.
- Forgetting recursive call-stack space.
- Applying the Master theorem when its recurrence form or regularity conditions do not fit.
- Treating the worst single operation as the amortized cost of every operation.
- Proving an invariant but never relating it to the stopping condition.

## 11. Questions and checks

**Q1.** Is 7n + 100 = O(n²)?  
**Answer:** Yes, though Θ(n) is the tighter description.

**Q2.** Does O(n) necessarily mean linear worst-case time?  
**Answer:** No. O is an upper-bound notation. State which case is being bounded.

**Q3.** How much work does a loop doing n, n/2, n/4, ... operations perform?  
**Answer:** It sums to less than 2n, so Θ(n).

**Q4.** T(n)=T(n/2)+1. What is the time?  
**Answer:** Θ(log n), because there is one constant-work call per halving level.

**Q5.** A recursive algorithm uses constant local variables per call. Is its auxiliary space O(1)?  
**Answer:** Not necessarily. The call stack can have many simultaneous frames.

## 12. Practice set

1. Give tight time bounds for a loop with i = 1; while i ≤ n, do constant work and set i = i+3.
2. Give tight time bounds for the same loop when i is multiplied by 3 each round.
3. Solve T(n)=2T(n/2)+n log n using a recursion tree.
4. State a loop invariant for finding the first occurrence of x in an array.
5. Explain why n calls to append in a doubling dynamic array have O(n) total copying work.
6. For a routine that stores n values and recursively processes n/2 values at a time, identify input and auxiliary memory separately.
7. Use induction to show 1+2+...+n = n(n+1)/2.

**Short solutions:** 1) Θ(n); 2) Θ(log n); 3) Θ(n log² n), since each of Θ(log n) levels contributes Θ(n log n) at the level's subproblem sizes in aggregate; 4) before checking i, if x occurs in A[0..i-1], the algorithm would already have returned, and all checked positions are not x; 5) the geometric sum of copied capacities is less than 2n; 6) state the recurrence and stack depth, then count explicitly allocated arrays; 7) base case n=1 and induction step adding n+1 to n(n+1)/2.

## Notebook work

From memory, draw (1) a recursion tree for merge sort, (2) a dynamic-array capacity timeline, and (3) the three loop-invariant proof steps. Label each with the cost or claim it proves.

**Next:** Chapter 2 develops arrays, searching, and sorting algorithms.
