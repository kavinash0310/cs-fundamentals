# Chapter 3 — Linked Lists, Stacks, Queues, and Deques

## Learning goals

Understand pointer-based lists, preserve their invariants during updates, and choose a stack, queue, deque, or array based on how data must be accessed.

## 1. Linked-list model

A singly linked node stores a value and a pointer to the next node. The head points to the first node; an empty list has head = NULL.

~~~text
head
 |
 v
+------+------+
|  14  | next |---->+------+------+
+------+------+     |  27  | next |---->+------+------+
                    +------+------+     |  39  | NULL |
                                         +------+------+
~~~

Nodes are not required to be adjacent in memory. Reaching position k requires following links, so indexing is Θ(n) worst case. Insertion after a node already in hand is Θ(1); searching for that node may take Θ(n).

| Operation | Time | Condition |
|---|---:|---|
| Read/insert/delete head | Θ(1) | Constant pointer changes |
| Search by value | Θ(n) | May inspect every node |
| kth element | Θ(n) | Follow links |
| Append with tail pointer | Θ(1) | Update tail and handle empty case |
| Append without tail pointer | Θ(n) | Find last node |
| Delete after known predecessor | Θ(1) | Relink |
| Delete by value | Θ(n) | Find predecessor |

A tail pointer speeds append but adds an invariant: tail is NULL exactly when the list is empty; otherwise it is the last node and tail.next is NULL.

## 2. Pointer updates

Insert node N after node P:

~~~text
N.next = P.next
P.next = N
~~~

Save the old link before overwriting it, or the rest of the list can become unreachable. To delete the next node, save it as victim, set P.next=victim.next, and release victim if memory is manually managed.

For head insertion, set N.next=head and then head=N. If the old list was empty, also set tail=N. Removing the only item resets both head and tail to NULL. Empty and one-node cases deserve explicit checks.

## 3. Doubly linked and circular lists

A doubly linked node stores prev and next:

~~~text
NULL <- [A] <-> [B] <-> [C] -> NULL
         head                 tail
~~~

Given a node, insertion/deletion can update neighbors in O(1), and backward traversal is possible. Costs are extra memory and more invariants: if x.next=y then y.prev=x, and adjacent links must agree. Head/tail boundaries need special handling; a sentinel can make boundaries more uniform.

A circular list links the final node back to the first. It supports cyclic traversal, but has no NULL endpoint. Stop after a complete cycle to avoid an infinite loop.

## 4. Stacks: last in, first out

Operations are push, pop, peek, and isEmpty. A stack returns the newest item first.

~~~text
push(4), push(9), push(2)

top -> +---+
       | 2 | <- pop returns 2
       +---+
       | 9 |
       +---+
       | 4 |
       +---+
~~~

A linked-list stack inserts/removes at the head in Θ(1). An array stack is also Θ(1) except occasional resizing; dynamic-array push is amortized Θ(1). Calling pop or peek on an empty stack is underflow and should be reported by the interface.

Applications: function-call frames, recursion, undo, depth-first search, delimiter matching, and expression evaluation.

**Delimiter trace:** Read the string ({[]}). Push (, {, [. The closing ] matches [ at the top; } then matches {; ) matches (. A mismatched top or leftover opener means invalid nesting. Closers must match the most recently unmatched opener, which is why the stack fits.

## 5. Queues: first in, first out

A queue inserts at the rear and removes at the front.

~~~text
dequeue <- front [ A ][ B ][ C ] rear <- enqueue
~~~

A linked queue with head and tail performs both operations in Θ(1). Removing the final item resets both pointers to NULL.

A circular array reuses slots by wrapping indexes modulo capacity C. With a size representation, empty means size=0, full means size=C. Enqueue writes at (head+size) mod C then increments size. Dequeue reads head, advances head to (head+1) mod C, then decrements size.

~~~text
index:     0    1    2    3    4
           +----+----+----+----+----+
           | E  | F  |    | C  | D  |
           +----+----+----+----+----+
head=3, size=4; next rear position = (head+size) mod C = 2
~~~

An alternative reserves one empty slot: full means advancing rear meets head, and usable capacity is C-1. Do not mix these conventions.

## 6. Deques and priority queues

A deque supports insertion/deletion at both ends. A circular array or doubly linked list supports ends in O(1). A monotonic deque can compute sliding-window minima or maxima in linear time.

A priority queue removes the highest/lowest priority, not the oldest. A binary heap is common (Chapter 5). FIFO order and priority order are different.

## 7. Queue using two stacks

Enqueue into an input stack. When the output stack is empty, transfer all input items to output; then pop output. Transfer reverses the order once, restoring FIFO removal.

Each item moves between stacks at most once, so n operations require O(n) total transfer work and operations are amortized O(1). One dequeue may still cost Θ(n).

## 8. Choosing a representation

| Need | Choice | Trade-off |
|---|---|---|
| Random indexing and dense scans | Array/dynamic array | Middle insert/delete shifts |
| Updates near known node | Linked list | Poor indexing and pointer overhead |
| Newest item first | Stack | Only top accessible |
| Arrival order | Queue | Only front/rear operations |
| Both ends | Deque | More invariants |
| Cache-friendly storage | Usually array | Depends on workload |

Linked lists are not automatically faster for insertion: they help when the location is already known, while finding it can dominate.

## 9. Common errors

- Overwriting a link before saving the rest of the list.
- Forgetting head/tail updates for empty and one-node cases.
- Assuming linked-list indexing is constant time.
- Forgetting modulo wraparound.
- Mixing circular-queue full/empty conventions.
- Ignoring underflow or memory ownership.
- Confusing LIFO and FIFO.

## 10. Questions

**Q1.** Why is head insertion O(1)?  
**Answer:** Constant pointer updates and no traversal.

**Q2.** Why does a tail pointer not make tail deletion O(1) in a singly list?  
**Answer:** Finding the predecessor still requires traversal.

**Q3.** What is the circular-list traversal risk?  
**Answer:** No NULL endpoint; stop after one cycle.

**Q4.** Why use a stack for nested delimiters?  
**Answer:** The last opener must be matched first.

**Q5.** Are all two-stack queue dequeues worst-case O(1)?  
**Answer:** No; one may transfer Θ(n), although amortized cost is O(1).

## 11. Practice with answers

1. Insert N after node P: what is the safe pointer-update order? **Answer:** N.next=P.next, then P.next=N.
2. A queue contains only one item and it is removed. **Answer:** head=NULL and tail=NULL.
3. Test (), braces, and square brackets for strings ({[]}), ([)], and ((. **Answer:** first valid; second fails when ) meets [; third leaves open symbols.
4. Find the predecessor of a singly linked list's tail using only head and tail. **Answer:** Θ(n).
5. Capacity 5: enqueue A,B,C; dequeue once; enqueue D,E,F. **Answer:** head=1, size=5; F occupies the wrapped slot 0.
6. Why may an array beat a linked list for end insertion? **Answer:** Dense storage, O(1) indexing, fewer pointers, often better cache locality.
7. State a doubly linked invariant. **Answer:** Adjacent forward/back links point back to each other consistently.

## Notebook work

Draw a list before/after insertion, reciprocal arrows in a doubly linked list, a circular queue after wraparound, and the delimiter stack after every character.

**Next:** Chapter 4 covers trees, traversals, and binary-search trees.
