# Chapter 3 — Graphs, Orders, Monoids, and Groups

## 1. Graph foundations

A graph G=(V,E) represents objects and pairwise relationships. In an undirected graph edges are unordered; directed edges are ordered. Degree counts incident edges; in a directed graph use in-degree and out-degree. Sum of degrees in an undirected graph is 2|E|.

A path joins vertices; a cycle returns to its start. A graph is connected when every pair has a path. A tree is connected and acyclic and has |V|-1 edges. A bipartite graph divides vertices into two sets so every edge crosses the division; a graph is bipartite iff it contains no odd cycle.

A matching is a set of edges with no shared endpoints. A vertex cover touches every edge. A coloring assigns colors so adjacent vertices differ; the chromatic number is the least number of colors.

~~~text
A --- B
| \   |
|  \  |
D --- C
Odd cycle A-B-C-D-A? Check edge set before claiming parity.
~~~

Always inspect the actual edge list before deciding bipartiteness. For a graph with a matching question, a matching covers vertices, while a vertex cover covers edges.

## 2. Connectivity and counting

In a connected undirected graph, at least |V|-1 edges are needed. A connected graph with exactly |V|-1 edges is a tree. Adding any edge to a tree creates one cycle; removing any tree edge disconnects it.

For a simple undirected graph, maximum edges are V(V-1)/2. For a simple directed graph without self-loops, maximum arcs are V(V-1).

Euler trail uses every edge exactly once. In a connected undirected graph, an Euler circuit exists iff every vertex has even degree; an Euler trail with two distinct endpoints exists iff exactly two vertices have odd degree. This differs from Hamiltonian path, which visits each vertex once.

## 3. Partial orders and lattices

A partial order is reflexive, antisymmetric, transitive. A Hasse diagram omits loops and transitive edges; lower elements are drawn below higher elements. A total order compares every pair, while a partial order may leave elements incomparable.

A lattice is a partially ordered set where every pair has a greatest lower bound (meet) and least upper bound (join). For subsets ordered by inclusion, meet is intersection and join is union. For divisibility on positive integers, gcd is meet and lcm is join where the relevant set is defined.

## 4. Monoids and groups

A semigroup has an associative binary operation. A monoid adds an identity element e with e*a=a*e=a. A group is a monoid where every element has an inverse. If operation is commutative, the group is abelian.

Examples:
- Integers under addition form an abelian group; identity 0 and inverse of a is -a.
- Positive integers under multiplication form a monoid with identity 1, but not a group because most elements lack inverses within the set.
- Integers modulo n under addition form a finite abelian group.
- Nonzero residues modulo prime p under multiplication form a group.

These structures describe algebraic laws, not necessarily numbers; strings under concatenation form a monoid with empty string identity.

## 5. Worked checks

For a graph with degrees 3,2,2,1, degree sum=8 so it has 4 edges. If connected with 4 vertices and 3 edges, it is a tree; with 4 edges it has a cycle.

For subsets of {a,b}, under inclusion: {a} and {b} are incomparable; their meet is ∅ and join is {a,b}.

## 6. Questions and practice

**Q1.** Is a tree with V vertices allowed to have V edges? **A:** No; a tree has V-1.
**Q2.** What characterizes bipartite graphs? **A:** Two-colorability, equivalently no odd cycle.
**Q3.** What is the operation/identity for strings under concatenation? **A:** Concatenation/empty string.
**Q4.** Is divisibility a total order? **A:** No; 2 and 3 are incomparable.
**Q5.** How many odd-degree vertices can an undirected graph have? **A:** An even number.
**Practice:** Determine whether small graphs are bipartite; find a maximum matching and a vertex cover; draw a Hasse diagram for divisors of 12; identify identity and inverses for modular addition/multiplication.

## Notebook work

Draw one tree, one odd cycle, and one Hasse diagram. Compare Euler and Hamiltonian questions in your own words.
