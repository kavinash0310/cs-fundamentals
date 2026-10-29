# Data Structures and Algorithms

A teach-from-the-page course on the core undergraduate data structures and algorithm-design topics used in GATE CS, computer-science teacher preparation, and product-company coding interviews. Work through chapters in order. Each chapter combines definitions, diagrams, worked traces, complexity analysis, common mistakes, questions, and practice with answers.

## Before you start

Recommended background: basic C programming, arrays, pointers, functions, and recursion. Review [Programming Fundamentals](programming-fundamentals.md) if pointers, array indexing, or recursive calls are unfamiliar. Chapter 1 revisits the complexity and proof tools used throughout.

## Chapter sequence

| Chapter | Main ideas |
|---|---|
| [1. Algorithm analysis and correctness](data-structures-algorithms/chapter-01-analysis-and-correctness.md) | Input-size models, O/Ω/Θ, cases, auxiliary space, amortized analysis, recurrences, loop invariants |
| [2. Arrays, searching, and sorting](data-structures-algorithms/chapter-02-arrays-searching-and-sorting.md) | Array costs, two pointers, prefix sums, linear/binary search, sorting traces, stability, comparison lower bound |
| [3. Linked lists, stacks, and queues](data-structures-algorithms/chapter-03-linked-lists-stacks-and-queues.md) | Singly/doubly/circular lists, pointer updates, stack/queue/deque, circular queues, applications |
| [4. Trees and binary-search trees](data-structures-algorithms/chapter-04-trees-and-binary-search-trees.md) | Tree properties, traversals, BST search/insert/delete, height, balanced-tree purpose, expression trees |
| [5. Heaps and hashing](data-structures-algorithms/chapter-05-heaps-and-hashing.md) | Binary-heap indexing and operations, build-heap, heapsort, priority queues, collisions, probing, load factor |
| [6. Graphs, BFS, DFS, and topological order](data-structures-algorithms/chapter-06-graphs-bfs-dfs-and-topological-order.md) | Matrix/list/edge-list representations, traversals, components, unweighted shortest paths, DAG ordering |
| [7. Greedy, divide and conquer, and graph optimization](data-structures-algorithms/chapter-07-greedy-divide-and-conquer-graph-optimization.md) | Exchange arguments, activity selection, recurrences, DSU, Kruskal, Prim, Dijkstra, Bellman-Ford |
| [8. Dynamic programming and review](data-structures-algorithms/chapter-08-dynamic-programming-and-review.md) | State design, knapsack, LCS, coin change, matrix chain, greedy comparison, mixed solved practice |

## How to study

For each chapter:

1. Read once for the model and vocabulary; copy the diagrams by hand.
2. Work every trace without looking at its answer.
3. Write the operation costs and the assumptions that make them true.
4. Solve the practice set, then explain one answer as if teaching a class.
5. Revisit missed questions after a day and again after a week.

For interview practice, narrate the input constraints, chosen representation, invariant, edge cases, and time/space cost before coding. For exam practice, show intermediate states and justify why each algorithm applies.

## Syllabus and exam alignment

The official GATE CS 2027 syllabus includes programming in C and data structures such as arrays, stacks, queues, linked lists, trees, binary search trees, binary heaps, and graphs; its algorithms section includes searching, sorting, hashing, asymptotic analysis, greedy methods, dynamic programming, divide and conquer, graph traversals, minimum spanning trees, and shortest paths. This course provides a structured treatment of that DSA core. Check the official syllabus for the attempt year and use the [GATE syllabus source](https://gate2027.iitm.ac.in/static/doc/GATE2027_Syllabus/CS_GATE2027_Syllabus.pdf).

For Bihar computer-teacher exams, the course builds definitions, worked examples, objective-question reasoning, and teaching explanations. Recruitment scope changes by post and cycle; confirm the current [BPSC syllabus](https://bpsc.bihar.gov.in/syllabus/) and the exact recruitment notice, including pedagogy and general studies where applicable.

This course is one part of the repo's larger CS map. It does not cover all GATE subjects, all interview formats, or every topic that a future teacher recruitment notice may require.
