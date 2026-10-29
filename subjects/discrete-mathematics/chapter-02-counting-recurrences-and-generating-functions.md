# Chapter 2 — Counting, Recurrences, and Generating Functions

## 1. Counting by choices

The sum rule adds counts of disjoint alternatives. The product rule multiplies counts of successive independent choices. If a task has r stages with mᵢ choices at stage i, total outcomes are ∏mᵢ.

Permutations of r distinct items from n: P(n,r)=n!/(n-r)!. Combinations of r items from n: C(n,r)=n!/(r!(n-r)!). Order matters for permutations, not combinations.

Example: choose a chair and vice-chair from 8 people: 8·7=56. Choose a two-person committee: C(8,2)=28.

With repetition, count carefully: length-r strings over n symbols allow nʳ outcomes. If order does not matter and repetition is allowed, the count is C(n+r-1,r).

## 2. Binomial identities and pigeonhole principle

The binomial theorem:
~~~text
(x+y)^n = Σ from k=0 to n C(n,k) x^(n-k)y^k
~~~
Pascal identity C(n,k)=C(n-1,k)+C(n-1,k-1) follows by separating selections that include a fixed element from those that do not.

Pigeonhole principle: placing N objects into M boxes with N>M forces some box to contain at least two. General form: some box contains at least ceil(N/M) objects. Use it to prove existence, not necessarily to construct the object.

Example: among 13 people, at least two share a birth month because 13 people enter 12 month-boxes.

## 3. Inclusion-exclusion

For two sets, |A∪B|=|A|+|B|-|A∩B|. For three sets, add singles, subtract pairwise intersections, then add the triple intersection.

Example: in a class of 40, 22 study C, 18 Python, and 9 both. At least one =22+18-9=31; neither =9.

## 4. Recurrence relations

A recurrence defines terms from earlier terms plus base cases. For T(n)=2T(n/2)+n, a recursion tree has n work per level and log₂n levels, so T(n)=Θ(n log n). For T(n)=T(n-1)+1, there are n levels and T(n)=Θ(n).

Linear recurrences can be solved with characteristic equations. For F(n)=F(n-1)+F(n-2), assume rⁿ: r²=r+1, roots (1±√5)/2. The closed form is dominated by φⁿ, where φ=(1+√5)/2.

### Master theorem reminder

For T(n)=aT(n/b)+f(n), compare f(n) with n^(log_b a). Equal-order f yields an extra logarithmic factor; smaller f leaves leaf work dominant; larger f can dominate if regularity conditions hold. State the case and check assumptions.

## 5. Generating functions

A sequence a₀,a₁,... has ordinary generating function A(x)=Σaₙxⁿ. The coefficient of xⁿ encodes aₙ. Generating functions turn recurrence relations into algebraic equations.

Example: 1,1,1,... has A(x)=1+x+x²+...=1/(1-x) for |x|<1. The sequence n has generating function x/(1-x)², obtained by differentiating the geometric series.

For Fibonacci numbers F₀=0,F₁=1, the recurrence yields F(x)=x/(1-x-x²). Exam problems often ask to identify a coefficient or derive the generating function rather than manipulate complex analysis.

## 6. Worked counting problem

How many binary strings of length 6 contain exactly two 1s? Choose which two positions contain 1: C(6,2)=15.

How many have no adjacent 1s? Let aₙ count length-n strings. Strings beginning with 0 leave aₙ₋₁ choices; those beginning with 10 leave aₙ₋₂ choices. Thus aₙ=aₙ₋₁+aₙ₋₂, with a₀=1,a₁=2, so a₆=21.

## 7. Questions and practice

**Q1.** How many ordered pairs of distinct people from n? **A:** n(n-1).
**Q2.** How many unordered pairs? **A:** C(n,2).
**Q3.** What does pigeonhole principle guarantee for 25 objects in 6 boxes? **A:** Some box has at least ceil(25/6)=5.
**Q4.** Solve T(n)=T(n/2)+1. **A:** Θ(log n).
**Q5.** Number of strings of length 5 with exactly three 1s? **A:** C(5,3)=10.
**Practice:** Count arrangements with repeated letters; derive an inclusion-exclusion count; solve a recurrence by tree and substitution; derive the recurrence for binary strings with no adjacent 1s and list first six values.

## Notebook work

Draw a recursion tree and a Pascal triangle. Label each counting problem with whether order and repetition matter.
