# Chapter 4 — Linear Algebra for Computer Science

## 1. Matrices and systems

A matrix is a rectangular array. Matrix addition is entrywise; multiplication AB is defined when columns of A equal rows of B. Matrix multiplication is associative but generally not commutative.

A linear system Ax=b asks for vector x. Row operations preserve the solution set: swap rows, multiply a row by a nonzero scalar, or add a multiple of one row to another. Gaussian elimination reduces an augmented matrix to echelon form; back substitution solves for pivots.

~~~text
x + y = 5
2x - y = 1
add equations: 3x=6 => x=2, y=3
~~~

A pivot in every variable gives a unique solution. A row [0 ... 0 | c], c≠0, means inconsistency. A free variable indicates multiple solutions if the system is consistent.

## 2. Determinants and invertibility

For a 2×2 matrix [[a,b],[c,d]], determinant is ad-bc. A square matrix is invertible iff determinant is nonzero, equivalently its rows/columns are independent and Ax=0 has only the zero solution.

Determinants multiply: det(AB)=det(A)det(B). Swapping rows changes sign; multiplying a row by c multiplies determinant by c; adding a multiple of one row to another leaves it unchanged.

For an upper triangular matrix, determinant is the product of diagonal entries. Use elimination carefully: track row swaps and row scaling.

## 3. Eigenvalues, eigenvectors, diagonalization

An eigenvector v≠0 satisfies Av=λv. Thus (A-λI)v=0 has a nonzero solution, so det(A-λI)=0. Solve this characteristic equation for eigenvalues, then solve for associated eigenspaces.

Example A=[[2,0],[0,3]] has eigenvalues 2 and 3 with eigenvectors along coordinate axes. Repeated eigenvalues may have fewer independent eigenvectors than their multiplicity.

If A has enough independent eigenvectors, A=PDP⁻¹. Then A^k=PD^kP⁻¹, making repeated transformations efficient to analyze.

## 4. LU decomposition

LU factorization writes A=LU, where L is lower triangular and U upper triangular (sometimes a permutation matrix is also needed). Solve Ax=b in two steps: Ly=b by forward substitution, then Ux=y by back substitution. Reusing the factorization is efficient for many right-hand sides.

## 5. Common mistakes and checks

- Matrix multiplication order and dimensions matter.
- A zero determinant means singular, not necessarily zero matrix.
- Eigenvectors cannot be the zero vector.
- A repeated eigenvalue does not guarantee diagonalizability.
- Keep row-swap and scaling effects when computing determinants.
- Distinguish a vector equation from an elementwise equation.

## 6. Questions and practice

**Q1.** When is a square matrix invertible? **A:** Nonzero determinant, full rank, equivalently a unique solution for every b.
**Q2.** Find determinant of [[4,1],[2,3]]. **A:** 12-2=10.
**Q3.** What equation gives eigenvalues? **A:** det(A-λI)=0.
**Q4.** Why factor A once for many vectors b? **A:** Reuse triangular solves instead of repeating elimination.
**Practice:** Solve a 3-variable system by elimination; find eigenvalues of a diagonal matrix; decide whether a given eigenbasis diagonalizes A; perform one LU solve.

## Notebook work

Perform Gaussian elimination by hand and annotate each row operation. Draw the relationship between determinant, rank, and unique solutions.


## Worked elimination and eigenvalue examples

Solve x+y=5 and 2x-y=1. Augmented matrix:
~~~text
[1  1 | 5]
[2 -1 | 1]
R2 <- R2 - 2R1:
[1  1 | 5]
[0 -3 |-9]
~~~
Thus y=3, x=2. The pivot in each variable gives a unique solution.

For A=[[2,0],[0,3]], characteristic equation is (2-λ)(3-λ)=0, so eigenvalues are 2 and 3. For λ=2, eigenvectors are multiples of (1,0); for λ=3, multiples of (0,1). These independent eigenvectors form P and diagonal matrix D.

If det(A)=0, columns are dependent and Ax=0 has a nonzero solution. If det(A)≠0, every b has exactly one x. Row reduction can solve a system even when determinant calculation is inconvenient.

**Practice:** Compute determinant of [[1,2],[3,4]] = -2. Explain why no inverse exists if determinant is zero. For a triangular matrix, multiply diagonal entries to find determinant.