# Chapter 1 — Computational Thinking and Python Foundations

## 1. Computational thinking
Decomposition breaks a task into parts; abstraction keeps relevant details; pattern recognition reuses structure; algorithms specify steps. A good classroom explanation makes input, process, output, and edge cases visible.

## 2. Python values and control
Python values have types: int, float, bool, str, list, tuple, dict. Variables refer to objects. Indentation groups blocks. Use if/elif/else for decisions, for for iteration over sequences, while for condition-controlled repetition.

~~~python
total = 0
for mark in marks:
    if mark >= 0:
        total += mark
~~~
The code sums non-negative marks. Trace each iteration with a table to teach state changes.

## 3. Functions and testing
A function names reusable behavior, takes parameters, and may return a value. Local variables belong to the call. Use small functions with clear contracts. Test typical, boundary, and invalid inputs.

## 4. Questions and practice
**Q1.** What is algorithm decomposition? **A:** Break task into manageable subproblems.
**Q2.** List mutability? **A:** Lists are mutable; tuples are immutable.
**Q3.** Why use a function? **A:** Reuse, naming, testing, and abstraction.
Practice: write a grade classifier; trace a loop; create a function for average with empty-input behavior.

## Notebook work
Write a flowchart and matching Python function for a classroom task.


## Trace a Python function

~~~python
def count_even(values):
    count = 0
    for value in values:
        if value % 2 == 0:
            count += 1
    return count
~~~
For [2,5,8], trace (value,count): (2,1), (5,1), (8,2); returns 2. Loop invariant: after processing the first i values, count equals number of even values in that prefix.

Python list aliasing: b=a does not copy; both names refer to the same mutable list. A shallow copy duplicates the outer list but nested objects may still be shared. This matters when explaining mutation to students.

**Practice:** Modify to return both even and odd counts; specify empty-input output; test negative even number -4; explain why -4%2==0.

## Extended lesson: trace a beginner program carefully

Computational thinking turns a problem into steps a computer can execute. Decomposition splits a task into smaller tasks; pattern recognition reuses known structure; abstraction keeps relevant facts; an algorithm specifies finite, unambiguous steps. A flowchart shows control visually, while pseudocode explains logic without committing to a programming language.

### Trace table: count values meeting a condition

~~~python
values = [4, 7, 2, 9]
count = 0
for x in values:
    if x > 5:
        count += 1
print(count)
~~~

| Iteration | x | x > 5 | count after iteration |
|---|---:|---|---:|
| 1 | 4 | false | 0 |
| 2 | 7 | true | 1 |
| 3 | 2 | false | 1 |
| 4 | 9 | true | 2 |

The loop invariant is: after processing the first k values, count equals the number of those k values greater than 5. At termination, k is the list length, so the printed count is correct. This is the same reasoning used to argue that a loop is correct in advanced algorithms.

### Variables, types, and aliasing

A variable name refers to a value. In Python, assignment binds a name; it does not always copy an object:

~~~python
a = [1, 2]
b = a
b.append(3)
~~~

Now a and b refer to the same list, so both display [1, 2, 3]. A new list copy such as b=a.copy() would make a separate outer list. This distinction explains many beginner surprises with mutable lists. Strings and integers are immutable values, so operations produce a new value rather than changing the existing object.

### Functions and decomposition

A function should have a clear purpose, meaningful parameters, and a useful return value. Separate input, computation, and output where possible. For example, a function that returns the maximum should not also print it unless printing is part of its stated purpose; returning supports reuse and testing.

### Practice with answers

1. In the trace above, how many values are greater than 5? Two.
2. What is the loop invariant in one sentence? Count matches qualifying items already visited.
3. Why does appending through b change a? Both names reference the same mutable list.
4. What makes an algorithm finite? It eventually terminates after a bounded sequence of steps for its input.
