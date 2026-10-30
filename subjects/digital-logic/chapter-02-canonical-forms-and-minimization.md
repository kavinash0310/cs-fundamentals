# Chapter 2 — Canonical Forms, Karnaugh Maps, and Minimization

## 1. Truth tables and canonical forms
An n-input Boolean function has 2ⁿ rows. A minterm is an AND term containing each variable once; it is 1 on exactly one row. A maxterm is an OR term containing each variable once; it is 0 on exactly one row.

Sum of minterms writes F=Σm(indices where F=1). Product of maxterms writes F=ΠM(indices where F=0). Keep variable order fixed when numbering rows.

Example: for variables A,B, F=1 on 01 and 10, so F=A'B+AB'=A XOR B.

## 2. Algebraic simplification
Use identities to combine terms. Consensus theorem: XY+X'Z+YZ = XY+X'Z; the consensus term YZ is redundant. Simplification aims to reduce gates, literals, delay, or power; the best result depends on implementation cost.

A truth table verifies equivalence exactly for small input counts. For larger functions, use Karnaugh maps or tabulation.

## 3. Karnaugh maps
K-map cells are arranged in Gray-code order so neighboring cells differ in one variable. Group 1s in powers of two (1,2,4,8...), using wraparound adjacency. Make groups as large as possible and cover every 1. Overlap is allowed if it reduces terms. A variable that changes within a group disappears; a variable fixed at 1 appears uncomplemented, fixed at 0 complemented.

For F(A,B,C)=Σm(1,3,5,7), all minterms have C=1, so F=C. The four cells form one group.

Don't-care entries may be treated as 0 or 1 to permit larger groups, but they need not be covered.

## 4. Tabular minimization
Quine–McCluskey combines minterms differing in one bit, replacing that bit by a dash. Repeat until no more combinations; prime implicants are uncombined terms. A prime-implicant chart selects a minimum cover. This method is systematic and suited to computer implementation; it grows costly as variable count increases.

## 5. Common mistakes
- Grouping diagonal K-map cells as adjacent.
- Using binary order instead of Gray-code order.
- Making groups of size 3 or 6.
- Omitting a minterm or including a zero.
- Assuming a don't-care must be used.
- Changing variable order midway.

## 6. Questions and practice
**Q1.** How many truth-table rows for 5 inputs? **A:** 32.
**Q2.** What is a minterm? **A:** Product term true on exactly one assignment.
**Q3.** Can K-map groups wrap around edges? **A:** Yes, opposite edges are adjacent.
**Q4.** Simplify Σm(0,2,4,6) for A,B,C with standard indexing. **A:** C' (even minterms).
Practice: derive Σm for XOR; minimize a 3-variable function from its table; identify a don't-care grouping; prove the result with a truth table.

## Notebook work
Draw the 3-variable Gray-code map and mark one group that wraps at an edge.


## Karnaugh-map worked method

For F(A,B,C)=Σm(0,2,5,7), minterms 0 and 2 group where A=0,C=0, yielding A'C'. Minterms 5 and 7 group where A=1,C=1, yielding AC. Thus F=A'C'+AC, the XNOR of A and C; B disappears.

Use Gray order for AB rows 00,01,11,10 and C columns 0,1. Opposite map edges are adjacent because they differ in one bit. A group of four removes two changing variables; group of two removes one.

For don't-care d, include it only if it enlarges a group or reduces implementation cost. Verify a minimized expression against original minterm set and don't-care freedom.

Quine-McCluskey example: minterms 000 and 001 differ only in last bit, combine to 00-. Repeat pairwise by number of ones; a dash position must match before combining.

**Practice:** Minimize XOR using minterms; identify essential prime implicant (one covering a minterm no other prime implicant covers); check a proposed grouping for adjacency and power-of-two size.