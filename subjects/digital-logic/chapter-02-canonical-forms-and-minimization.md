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

## Extended lesson: minimize with a three-variable K-map

A Karnaugh map arranges minterms so horizontally and vertically adjacent cells differ in exactly one variable. For three variables A,B,C, Gray ordering for BC columns is 00, 01, 11, 10, not ordinary binary order.

Consider F(A,B,C)=Σm(1,3,5,7). These are binary 001,011,101,111: every cell has C=1. Group all four cells as one rectangle, eliminating A and B, so F=C. Group sizes must be powers of two, and wraparound edges are adjacent. Groups may overlap if that reduces the number of literals.

### SOP and POS

For SOP, group 1-cells; each group yields a product term containing only variables fixed throughout the group. A variable fixed at 1 appears uncomplemented; fixed at 0 appears complemented. For POS, group 0-cells; each group yields a sum term. Use don't-care entries only when assigning them 0 or 1 helps produce larger groups; the circuit may choose either value for those input combinations.

A second example: F=Σm(0,2,4,6) is the complement of the previous set, so F=C'. Recognizing complement patterns often saves work.

### Quine–McCluskey connection

For many variables, tabulate minterms by count of 1s, combine terms differing in one bit, and mark combined terms. Uncombined terms are prime implicants. Choose a minimum cover that includes every 1-minterm. K-maps and Quine–McCluskey are both ways to find implicant groupings; the tabular method scales more systematically.

### A reliable exam procedure

1. Fix variable order and map labels before placing values.
2. Mark minterms and don't-cares.
3. Make largest legal groups first; allow wraparound and overlap.
4. Confirm every 1 is covered.
5. Write the simplified expression and verify it against the original minterms.

### Questions

- Why are columns 00,01,11,10? Adjacent columns differ in one bit.
- Can a group contain three cells? No, group cardinality must be a power of two.
- When should a don't-care be used? Only if it helps simplification; otherwise treat it as 0 for SOP.
