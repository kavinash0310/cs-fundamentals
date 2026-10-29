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
