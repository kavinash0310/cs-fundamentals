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

## 5. Worked mini-project: class score summary

**Problem:** Given a list of quiz marks, report the number of valid marks, the average, and how many passed with mark >=40. State how the program behaves for an empty list and invalid marks.

### Decompose before coding

~~~text
input marks
  -> validate each mark is in [0, 100]
  -> accumulate total, valid_count, pass_count
  -> if valid_count is 0, report "no valid marks"
     otherwise report total / valid_count and pass_count
~~~

A useful loop invariant is: after processing the first i entries, total equals the sum of valid marks among those i entries, valid_count equals their number, and pass_count equals the number at least 40. At the end, the accumulator describes the entire list.

~~~python
def summarize(marks):
    total = 0
    valid_count = 0
    pass_count = 0

    for mark in marks:
        if not isinstance(mark, (int, float)) or mark < 0 or mark > 100:
            continue
        total += mark
        valid_count += 1
        if mark >= 40:
            pass_count += 1

    if valid_count == 0:
        return None
    return total / valid_count, pass_count
~~~

For [35, 40, 80], the trace is:

| mark | valid? | total | valid_count | pass_count |
|---:|---|---:|---:|---:|
| 35 | yes | 35 | 1 | 0 |
| 40 | yes | 75 | 2 | 1 |
| 80 | yes | 155 | 3 | 2 |

The result is average 155/3≈51.67 and 2 passes. For [], the function returns None. In production code, silently skipping invalid values may hide data errors; the specification should decide whether to reject the whole input or report invalid entries.

### Control flow and common errors

- if/elif/else chooses one branch; conditions are checked top to bottom.
- A for loop visits each item in a sequence; a while loop repeats while a condition remains true.
- Update the loop variable or state so a while loop eventually stops.
- In Python, = assigns a value; == compares values.
- and requires both conditions; or requires at least one; not reverses truth.
- Python indentation is syntax. A block must use consistent indentation.

Boundary tests: marks 0 and 100 are valid; −1 and 101 are invalid; 39 fails and 40 passes. These tests verify both the input limits and the inclusive threshold.

### Teaching note: use the same idea in three representations

An algorithm can be taught with a sentence, a flowchart, pseudocode, and runnable code. They should express the same decisions.

~~~text
            +------------------+
            | read next mark   |
            +--------+---------+
                     |
              valid mark?
               /          \
             no            yes
             |              |
         report/skip    add to total
                            |
                       mark >= 40?
~~~

Before showing code, ask learners what state must be remembered between marks. That state becomes the accumulator variables. After coding, ask them to predict one trace row before running the program; this reveals whether they understand state changes rather than merely recognize syntax.

**Questions with reasoning**

1. Why is the empty list a boundary case? The average would divide by zero unless the program defines a behavior.
2. Why use >= 40 instead of > 40? The stated pass rule includes a score exactly equal to 40.
3. What does the invariant establish? The accumulators mean what the algorithm claims at every iteration, so the final result follows after the last item.
4. Why test 39 and 40 together? They lie on opposite sides of the decision boundary and expose an off-by-one condition.
5. When should invalid inputs be rejected instead of skipped? When silently ignoring them could make a reported summary incomplete or misleading.
