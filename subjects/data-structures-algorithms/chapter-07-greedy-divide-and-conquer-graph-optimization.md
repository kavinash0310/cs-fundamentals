# Chapter 7 — Greedy Methods, Divide and Conquer, and Graph Optimization

## Learning goals

Recognize when a locally best choice can be proved globally optimal, apply Kruskal and Prim to minimum spanning trees, select a shortest-path algorithm based on edge weights, and analyze divide-and-conquer recurrences.

## 1. Greedy algorithm design

A greedy algorithm repeatedly makes a locally attractive choice and commits to it. This is not automatically correct. A proof needs to show that some optimal solution can be transformed to include the greedy choice without becoming worse, and that the remaining subproblem has the same structure.

Common proof tools:

- **Exchange argument:** replace an optimal solution's choice with the greedy choice and show feasibility and no larger cost.
- **Cut property:** for a cut of a graph, a minimum-weight edge crossing that cut is safe for a minimum spanning tree (when handled consistently with ties).
- **Stays-ahead argument:** after each step the greedy partial solution is at least as good as any competitor under a defined measure.

### Activity selection

Each activity has start and finish times; choose the largest mutually compatible set. The greedy rule “choose the compatible activity that finishes earliest” is optimal.

Proof sketch: let g be the activity that finishes earliest, and let a₁ be the first activity in an optimal solution. Since finish(g) ≤ finish(a₁), replacing a₁ with g leaves at least as much time for all later selected activities. Thus some optimal solution begins with g. Repeat on the remaining compatible activities.

Choosing earliest start, shortest duration, or least overlap is not generally a valid replacement rule.

## 2. Divide and conquer

A divide-and-conquer algorithm splits a problem into smaller subproblems, solves them recursively, and combines their answers. Its recurrence often has the form T(n)=aT(n/b)+f(n).

Examples:

- Binary search: one half remains and constant extra work; Θ(log n).
- Merge sort: two halves plus linear merge; Θ(n log n).
- Quicksort: partitions then solves two unequal sides; average/expected Θ(n log n), worst Θ(n²) for highly unbalanced splits.
- Maximum and minimum: split array, combine extrema; Θ(n) comparisons up to constants.

A recurrence tree helps reveal total work per level. Master theorem applies only when the recurrence matches its hypotheses; refer to Chapter 1.

## 3. Minimum spanning trees

For a connected, undirected, weighted graph, a spanning tree connects every vertex using V-1 edges and has no cycles. A minimum spanning tree (MST) has minimum total edge weight. If the graph is disconnected, the analogous result is a minimum spanning forest.

### Kruskal's algorithm

Sort edges by nondecreasing weight. Add an edge if it connects two currently separate components; skip it if it creates a cycle. Stop after V-1 edges for a connected graph.

A disjoint-set union structure (DSU/union-find) supports:

- make-set(v): create a component for v.
- find(v): identify its representative.
- union(a,b): merge two components.

With path compression and union by rank/size, a sequence of operations is near-constant amortized time, formally O(α(V)) per operation in the standard analysis, where α is the inverse Ackermann function.

~~~text
edges in weight order: AB:1, BC:2, AC:3, CD:4, BD:5
take AB; take BC; skip AC (would form cycle); take CD
MST edges = {AB, BC, CD}, total weight = 7
~~~

Sorting dominates: O(E log E), commonly written O(E log V) for simple graphs. The cut property justifies accepting a light edge that safely joins components.

### Prim's algorithm

Start from any vertex and repeatedly add a minimum-weight edge connecting the current tree to an outside vertex. A min-priority queue tracks each outside vertex's cheapest connection. It grows one tree; for a connected graph it finishes with an MST.

With adjacency lists and a binary heap, the standard implementation runs in O((V+E) log V). An adjacency matrix implementation can run in O(V²), useful for dense graphs.

Prim and Kruskal both produce an MST, but their mechanics differ: Prim grows one connected tree; Kruskal merges a forest of components.

## 4. Shortest paths

A shortest path minimizes total edge weight from a source to each reachable vertex. Do not confuse this with the fewest number of edges.

### Dijkstra's algorithm

Dijkstra works when all edge weights are non-negative. Initialize dist[source]=0 and other distances to infinity. Repeatedly take the unsettled vertex with the smallest tentative distance, then relax each outgoing edge (u,v,w):

~~~text
if dist[u] + w < dist[v]:
    dist[v] = dist[u] + w
    parent[v] = u
~~~

The greedy step is safe because a path leaving the selected vertex cannot later become cheaper via non-negative edges. With adjacency lists and a binary heap, typical complexity is O((V+E) log V). A simple array/matrix version is O(V²).

**Trace:** Edges S→A (4), S→B (1), B→A (2), A→T (1), B→T (7). Start dist(S)=0. Relax S: A=4, B=1. Select B: improve A=3, set T=8. Select A: improve T=4. Final shortest route to T is S→B→A→T, cost 4.

A negative edge invalidates Dijkstra's finalization argument, even if there is no negative cycle. Do not use it when negative weights are allowed.

### Bellman-Ford

Bellman-Ford supports negative edge weights. Initialize distances as above, then relax every edge repeatedly V-1 times. A shortest simple path uses at most V-1 edges; after that, one more pass that can still improve a reachable distance indicates a reachable negative-weight cycle.

Time O(VE), space O(V). Unreachable negative cycles do not affect distances from the selected source. If a reachable negative cycle can be traversed repeatedly, there is no finite shortest distance for vertices reachable from it.

### Comparing shortest-path choices

| Graph condition / need | Algorithm |
|---|---|
| Unweighted or equal-weight edges; minimize edge count | BFS |
| Non-negative weights; one source | Dijkstra |
| Negative edges may occur; detect reachable negative cycle | Bellman-Ford |
| All-pairs shortest paths | Floyd-Warshall is a common option, Θ(V³) |

## 5. Why MST and shortest paths differ

An MST minimizes the sum of selected edges connecting all vertices. It is not necessarily a shortest-path tree from any root. A shortest-path tree minimizes each source-to-vertex route; its total edge weight need not be minimum among all spanning trees.

Example: a triangle with edge weights AB=2, AC=2, BC=1. An MST chooses BC and one edge of weight 2, total 3. From A, shortest paths to B and C both have weight 2 directly. A shortest-path tree rooted at A uses AB and AC, total 4.

## 6. Common errors

- Assuming greedy is correct because it seems intuitive.
- Confusing a spanning tree with an MST.
- Adding an edge in Kruskal without checking whether it creates a cycle.
- Using Dijkstra with negative edges.
- Using BFS for non-uniform weights.
- Treating an undirected negative edge as harmless; traversing it back and forth creates a negative cycle.
- Checking Bellman-Ford cycles without restricting to vertices reachable from the source.
- Calling an MST a shortest-path tree.

## 7. Questions and answers

**Q1.** Why is earliest-finish greedy correct for activity selection? **Answer:** Exchange the first chosen optimal activity with the earlier-finishing greedy activity, preserving room for the rest.
**Q2.** What data structure makes Kruskal cycle checks efficient? **Answer:** DSU/union-find.
**Q3.** Does Dijkstra permit negative edges? **Answer:** No; non-negative weights are required.
**Q4.** How many edges are in a spanning tree on V vertices? **Answer:** V-1.
**Q5.** How many full edge-relaxation rounds are needed by Bellman-Ford before its negative-cycle check? **Answer:** V-1.
**Q6.** Is an MST necessarily a shortest-path tree? **Answer:** No; they optimize different quantities.

## 8. Practice with worked answers

1. Kruskal's edges are AB:1, BC:2, AC:3, CD:4. Which are selected? **Answer:** AB, BC; skip AC because it closes a cycle; select CD.
2. A graph has 8 vertices. How many edges must a spanning tree have? **Answer:** 7.
3. Can Dijkstra solve a graph with all weights zero or positive? **Answer:** Yes.
4. A reachable negative cycle exists. What does it mean for a vertex reachable from that cycle? **Answer:** Its path cost can be reduced without bound; no finite shortest distance exists.
5. Give one counterexample to the rule “choose the smallest edge incident to the current vertex” for building an MST. **Answer:** Greedy local selection can choose several cheap edges inside one region and defer a necessary connection; use the cut-safe rule and maintain cycle checks rather than an unproved local rule.
6. Why does Kruskal sort edges? **Answer:** It considers candidate safe edges in nondecreasing cost order.
7. Give the difference between Prim and Kruskal. **Answer:** Prim grows one tree; Kruskal merges components.

## Notebook work

Trace both MST algorithms on the same weighted graph. For shortest paths, draw a Dijkstra distance table and a Bellman-Ford pass table. Write the precondition beside each algorithm.

**Next:** Chapter 8 builds dynamic programs and ends with a mixed practice set.
