# Chapter 4 — Trees, Traversals, and Binary Search Trees

## Learning goals

Learn tree properties, trace recursive and iterative traversals, perform BST operations, and explain why balancing is needed for logarithmic worst-case costs.

## 1. Tree vocabulary

A rooted tree has a distinguished root. Every non-root node has exactly one parent; a leaf has no children. Depth counts edges from root. Height is the longest downward path to a leaf. State whether height counts edges or levels.

~~~text
               A                 root, depth 0
             /   \
            B     C               depth 1
           / \     \
          D   E     F             depth 2; leaves
~~~

A tree with n nodes has n-1 edges. A binary tree has at most two children, with left/right positions distinct. A **full** binary tree gives each node 0 or 2 children. A **complete** tree has all levels full except perhaps its last, filled left to right. A **perfect** tree is full with all leaves at the same depth.

At depth d, a binary tree has at most 2^d nodes. A perfect tree of height h, with root depth 0, has 2^(h+1)-1 nodes. If n₀ counts leaves, n₁ one-child nodes, and n₂ two-child nodes, then n₀=n₂+1. Proof: edges equal n-1 and also n₁+2n₂; equate and simplify.

## 2. Traversals

~~~text
               A
             /   \
            B     C
           / \     \
          D   E     F
~~~

- Preorder (node, left, right): A, B, D, E, C, F.
- Inorder (left, node, right): D, B, E, A, C, F.
- Postorder (left, right, node): D, E, B, F, C, A.
- Level order (breadth-first queue): A, B, C, D, E, F.

~~~text
inorder(node):
    if node == NULL: return
    inorder(node.left)
    visit(node)
    inorder(node.right)
~~~

Each traversal takes Θ(n). Recursive traversal uses O(h) call-stack space: Θ(log n) height for a balanced tree and Θ(n) for a skewed tree.

Iterative inorder pushes a left path, pops and visits, then moves right. Continue while the current node exists or the stack is non-empty. Level order uses a queue and adds children after visiting a node.

Inorder traversal of a BST produces sorted keys. On a general binary tree, it is just a traversal order.

## 3. Binary-search-tree invariant

For distinct keys, every key in a node's left subtree is smaller, and every key in its right subtree is larger. For duplicates, choose a consistent policy (for example, count duplicates in the node) and apply it to every operation.

~~~text
                 40
               /    \
             18      63
            /  \    /  \
           9   27  51   77
~~~

Search for 51: compare to 40, go right; compare to 63, go left; find it. Search, insertion, min/max, successor, and deletion take O(h).

If height is Θ(log n), operations are logarithmic. A plain BST can degenerate to height Θ(n), such as when sorted keys are inserted in order, making operations linear. A BST is not automatically balanced.

## 4. Search, insertion, and deletion

Search compares at each node and chooses one subtree; the BST property proves the other subtree cannot contain the key. Insertion follows the same path to an empty child and attaches the new key as a leaf.

Deletion has three cases:

1. **Leaf:** unlink it.
2. **One child:** replace it with the child.
3. **Two children:** replace with its inorder successor (minimum in right subtree) or predecessor (maximum in left), then delete that replacement from its old position.

Example: delete 40 from the drawn tree. Its successor is 51. Place 51 at the root, then remove 51 from its old position below 63. Reconnect the replacement subtree; handle root deletion explicitly in code.

## 5. Minimum, maximum, successor

Minimum follows left links until none remains; maximum follows right links. A node's successor is the minimum of its right subtree if one exists. Otherwise move upward to the first ancestor reached from its left side. If there is no such ancestor, the node is the maximum. Predecessor is symmetric.

## 6. Balanced trees and rotations

AVL trees maintain a strict subtree-height difference bound and rotate after updates. Red-black trees maintain color constraints that give a logarithmic height bound. Both provide O(log n) search, insertion, and deletion in standard implementations.

A rotation changes local links while preserving inorder order:

~~~text
      x                 y
       \               /
        y     ==>     x
       /               \
      T2                T2
~~~

The full rotation must reconnect every parent and subtree pointer. For GATE-level reasoning, understand height guarantees and why rotations preserve the sorted order; detailed balance cases depend on the syllabus.

## 7. Expression trees

Operators are internal nodes; operands are leaves. For (a+b)*c:

~~~text
        *
       / \
      +   c
     / \
    a   b
~~~

Preorder gives prefix: * + a b c. Inorder gives infix with parentheses as needed. Postorder gives postfix: a b + c *. To evaluate postfix, push operands; for each operator pop operands in order, apply, and push the result.

## 8. Worked traversal and height

~~~text
          8
        /   \
       3     10
      / \
     1   6
        / \
       4   7
~~~

Inorder: 1, 3, 4, 6, 7, 8, 10.  
Preorder: 8, 3, 1, 6, 4, 7, 10.  
Postorder: 1, 4, 7, 6, 3, 10, 8.  
Level order: 8, 3, 10, 1, 6, 4, 7.  
Height: 3 edges or 4 levels.

## 9. Common errors and checks

- Do not confuse full, complete, and perfect.
- Never claim O(log n) for a plain BST without a height assumption.
- Specify a duplicate-key policy.
- When deleting a two-child node, also delete its successor at the original location.
- Recursive traversal needs O(h) stack space.
- Check whether a question defines height in edges or levels.

**Q1.** What is inorder traversal of a BST? **A:** Sorted key order.
**Q2.** Worst-case search in an ordinary BST? **A:** Θ(n).
**Q3.** Which traversal naturally uses a queue? **A:** Level order.
**Q4.** A tree has 11 leaves and 4 one-child nodes. Find n₂ and n. **A:** n₂=10, n=25.

## 10. Practice with solutions

1. Insert 30,15,50,10,20,40,60,18. **Solution:** 30 root; 15 left with 10 and 20 (18 beneath 20); 50 right with 40 and 60.
2. Give its inorder and preorder. **Answer:** inorder 10,15,18,20,30,40,50,60; preorder 30,15,10,20,18,50,40,60.
3. Delete 15 using successor. **Answer:** replace it with 18 and remove old 18 under 20.
4. Give a height n-1 insertion order. **Answer:** sorted keys into a plain BST.
5. Evaluate postfix 5 2 3 * +. **Answer:** 11.
6. Recursive traversal stack for balanced/skewed tree? **Answer:** O(log n) / O(n).
7. Why can rotation preserve sorted order? **Answer:** It rearranges links among key ranges that already satisfy the BST ordering.

## Notebook work

Draw the three deletion cases, label key ranges around a BST node, and reproduce each traversal. Explain why tree height determines operation cost.

**Next:** Chapter 5 covers heaps, priority queues, and hashing.
