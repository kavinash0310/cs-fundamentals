# Chapter 6 — Graphs, BFS, DFS, and Topological Ordering

## Learning goals

Model relationships as graphs, choose an appropriate representation, trace breadth-first and depth-first search, analyze their costs, and detect when topological order exists.

## 1. Graph vocabulary

A graph G=(V,E) has vertices V and edges E. In an undirected graph, an edge {u,v} has no direction; in a directed graph it is an ordered pair (u,v). A weighted edge carries a cost or length.

A path is a sequence of vertices connected by edges. A cycle begins and ends at the same vertex. A graph is connected if every pair of vertices has a path between them. A tree is connected and acyclic. In a directed graph, distinguish reachability, strongly connected components, and weak connectivity.

~~~text
A --- B       D
|   /         |
| /           E
C             |
              F

component 1: {A,B,C}; component 2: {D,E,F}
~~~

Degree in an undirected graph is the number of incident edges. In a directed graph, distinguish in-degree and out-degree.

## 2. Graph representations

### Adjacency matrix

A V by V matrix stores whether an edge exists (or its weight). Space is Θ(V²); checking an edge is O(1); listing neighbors of one vertex costs Θ(V).

### Adjacency list

Each vertex stores its neighbors. Space is Θ(V+E) for an undirected graph up to constant factors; listing neighbors is proportional to degree. Checking a particular edge may take O(degree(v)).

| Operation | Matrix | Adjacency list |
|---|---:|---:|
| Space | Θ(V²) | Θ(V+E) |
| Check edge (u,v) | O(1) | O(degree(u)) |
| Iterate neighbors of u | Θ(V) | Θ(degree(u)) |
| Best fit | Dense graphs | Sparse graphs |

An edge list stores the edges as pairs/triples and is useful for algorithms that process edges globally, such as Kruskal's algorithm.

## 3. Breadth-first search

BFS explores by distance in number of edges from a source. It uses a FIFO queue and marks vertices when enqueued, preventing repeated enqueues.

~~~text
BFS(G, source):
    mark source; distance[source]=0
    enqueue(source)
    while queue not empty:
        u = dequeue()
        for each neighbor v of u:
            if v not visited:
                mark v
                distance[v]=distance[u]+1
                parent[v]=u
                enqueue(v)
~~~

With adjacency lists, one source traversal costs Θ(V+E) over the visited component; initializing all vertices adds Θ(V). With an adjacency matrix it costs Θ(V²). BFS computes shortest path lengths in an unweighted graph because vertices leave the queue in nondecreasing distance order.

**Trace:** Edges A-B, A-C, B-D, C-E; start at A and inspect neighbors alphabetically.

~~~text
queue      visited order
[A]        A
[B,C]      A
[C,D]      A,B
[D,E]      A,B,C
[E]        A,B,C,D
[]         A,B,C,D,E
~~~

Parents yield shortest paths: path to D is A→B→D, of length 2.

For a disconnected graph, BFS from one source reaches only its component. To traverse a whole graph, start a BFS from each still-unvisited vertex; total cost with adjacency lists is Θ(V+E).

## 4. Depth-first search

DFS follows one path as far as possible, then backtracks. It uses recursion or an explicit stack. It supports connected components, cycle detection, topological sorting, and many structural analyses.

~~~text
DFS(u):
    mark u
    for each neighbor v of u:
        if v is unvisited:
            parent[v]=u
            DFS(v)
~~~

With adjacency lists, full DFS is Θ(V+E); recursive stack space is O(V) worst case. Traversal order depends on neighbor ordering. The visited mark must be set before exploring neighbors, or cycles can recurse forever.

For undirected graphs, an already-visited neighbor other than the parent can indicate a cycle. Directed-cycle detection needs recursion-state colors: white (unseen), gray (active), black (finished). An edge to a gray node is a back edge and indicates a directed cycle.

## 5. BFS versus DFS

| Question | BFS | DFS |
|---|---|---|
| Main structure | Queue | Stack / recursion |
| Shortest path in unweighted graph | Yes | Not guaranteed |
| Deep exploration / structure | Less natural | Natural |
| Space | Can be Θ(V) | Stack may be Θ(V) |
| Typical uses | Levels, fewest edges | Components, cycle checks, ordering |

Neither is universally faster for the same adjacency-list traversal; both are Θ(V+E). The needed property selects the traversal.

## 6. Topological ordering

A topological ordering of a directed graph places u before v for every directed edge u→v. It exists exactly when the directed graph is acyclic (a DAG).

### Kahn's algorithm

Compute in-degree of each vertex, enqueue all vertices with in-degree zero, then repeatedly remove one, append it to the result, and decrement the in-degree of its outgoing neighbors. If fewer than V vertices are output, the graph contains a cycle.

~~~text
A -> C <- B
     |
     v
     D
~~~

One valid order is A,B,C,D or B,A,C,D. Independent vertices can appear in either order.

With adjacency lists, Kahn's algorithm is Θ(V+E). DFS also produces a topological order by reversing finishing times, provided the graph is a DAG.

## 7. Connected components and shortest paths

Repeated BFS/DFS finds connected components in an undirected graph. In a directed graph, strongly connected components are maximal groups where every vertex can reach every other; specialized algorithms appear in more advanced graph study.

BFS gives minimum-edge paths in unweighted graphs. For weighted edges, use an algorithm that respects weights: Dijkstra for non-negative weights, Bellman-Ford when negative edges may occur (if no reachable negative cycle makes shortest paths undefined). These are covered in Chapter 7.

## 8. Common errors

- Using V² space for a graph with millions of vertices and few edges.
- Forgetting whether E counts directed arcs or undirected edges.
- Marking visited only after dequeue and enqueuing duplicates unnecessarily.
- Claiming DFS finds shortest paths.
- Running BFS from one source and assuming disconnected vertices were visited.
- Forgetting the parent edge when checking an undirected cycle.
- Calling a directed graph acyclic without testing for back edges or incomplete topological output.

## 9. Questions and answers

**Q1.** Which representation suits a sparse graph? **Answer:** Adjacency lists.
**Q2.** What is BFS distance measured in an unweighted graph? **Answer:** Number of edges from the source.
**Q3.** Is DFS order unique? **Answer:** No; it depends on neighbor ordering.
**Q4.** When does a topological order exist? **Answer:** Exactly when a directed graph is acyclic.
**Q5.** What is adjacency-list traversal time? **Answer:** Θ(V+E) for a full graph traversal.

## 10. Practice with answers

1. A graph has V=1000 and E=1200. Which representation is usually more space-efficient? **Answer:** Adjacency list, Θ(V+E) rather than Θ(V²).
2. In an undirected graph, what is the sum of degrees? **Answer:** 2E, because each edge contributes to two endpoints.
3. In a directed graph, what is sum of in-degrees and sum of out-degrees? **Answer:** Each equals E.
4. Does BFS give a minimum-weight route when edge weights differ? **Answer:** No, it minimizes edge count only when edges are unweighted/equal cost.
5. Kahn's algorithm emits 7 vertices from a graph with 8. What follows? **Answer:** A directed cycle exists.
6. What is the traversal cost for a graph with adjacency matrix? **Answer:** Θ(V²) for standard BFS/DFS neighbor scans.
7. Why mark on enqueue in BFS? **Answer:** Each vertex enters the queue once and receives its shortest unweighted distance on discovery.

## Notebook work

Draw matrix and list representations for the same graph. Trace BFS and DFS with an explicit neighbor order, then run Kahn's algorithm and show in-degrees after each removal.

**Next:** Chapter 7 covers greedy algorithms, divide and conquer, spanning trees, and weighted shortest paths.
