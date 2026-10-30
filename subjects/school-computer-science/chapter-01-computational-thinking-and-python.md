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