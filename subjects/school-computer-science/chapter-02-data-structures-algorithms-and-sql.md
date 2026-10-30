# Chapter 2 — Data, Algorithms, and Introductory SQL

## 1. Collections
Lists preserve order and allow indexing; tuples are immutable sequences; dictionaries map keys to values; sets store unique elements. Choose by required operations, not habit. A dictionary lookup is expected fast under normal hashing assumptions.

A stack is LIFO; a queue is FIFO. Teach with a tray and line-of-people analogy, then test edge cases and complexity.

## 2. Algorithm tracing
A trace table records variables after each line/iteration. Linear search checks items one by one. Binary search halves a sorted range; it requires sorted input. Sorting orders records by a key. Discuss time cost using input length n and compare growth rather than wall-clock timing alone.

## 3. Relational data and SQL
A table has rows and columns; primary key identifies a row; foreign key references another table. SQL SELECT retrieves data, WHERE filters rows, JOIN combines related tables, GROUP BY aggregates.

~~~sql
SELECT class_id, COUNT(*)
FROM Student
GROUP BY class_id;
~~~
Explain that COUNT(*) counts rows; NULL behavior differs for COUNT(column).

## 4. Questions and practice
**Q1.** Which Python collection maps keys to values? **A:** Dictionary.
**Q2.** What precondition does binary search need? **A:** Sorted data.
**Q3.** WHERE vs GROUP BY? **A:** Filter rows vs form groups.
Practice: trace a search; count word frequencies with a dictionary; query students by class; explain primary/foreign key with a school example.

## Notebook work
Draw a table relationship and write one query that joins it.


## Trace search and SQL examples

Linear search invariant: before checking index i, all previous entries differ from target. Once equal, return; if loop ends, target is absent. Cost worst-case n comparisons.

SQL: find classes with at least 20 students:
~~~sql
SELECT class_id, COUNT(*) AS n
FROM Student
GROUP BY class_id
HAVING COUNT(*) >= 20;
~~~
The grouping occurs before HAVING. If two tables are joined, teach primary/foreign keys and show the row multiplication in an M:N join.

**Practice:** Trace binary search on [2,5,8,11,15] for 11; write a join from Student to Class; explain why sorting precondition matters; calculate total array traversals for nested loops.

## Extended lesson: choose a structure and query data

An array/list supports indexed access in constant time in typical implementations, while insertion near the front may shift many items. A stack is last-in, first-out (undo history); a queue is first-in, first-out (printer jobs). A set supports membership and uniqueness; a dictionary maps keys to values. Pick a structure from the operations the problem needs, not just from its name.

### Search and sort

Linear search checks up to n elements, so worst-case time is O(n). Binary search requires sorted data and a way to access the middle efficiently; each comparison halves the search interval, giving O(log n) comparisons. For n=16, at most about 4 halvings locate a value or establish absence.

Selection sort repeatedly selects the smallest remaining item and runs in O(n²) comparisons. Merge sort splits, sorts, and merges; its recurrence T(n)=2T(n/2)+O(n) gives O(n log n) time and it typically needs extra memory. Stability means equal-key records retain their original relative order.

### SQL example

Suppose Student(student_id, name, class_id) and Marks(student_id, subject, score). To list students whose score is at least 80:

~~~sql
SELECT s.name, m.subject, m.score
FROM Student AS s
JOIN Marks AS m ON m.student_id = s.student_id
WHERE m.score >= 80
ORDER BY s.name;
~~~

The join condition connects rows about the same student; WHERE filters individual result rows; ORDER BY controls presentation. A primary key uniquely identifies a row. A foreign key records a reference to another table's key and helps prevent orphan records.

### Check your understanding

1. Which search needs sorted data? Binary search.
2. What does a queue remove first? The earliest inserted element.
3. Does O(n log n) always beat O(n²) for tiny input? No; constants and input size matter.
4. Why use a join instead of storing a repeated student name in every Marks row? It reduces inconsistency and links each mark to one Student record.

## 5. Worked algorithm traces

### Linear and binary search

Search the sorted list [3, 8, 12, 17, 24, 31, 45] for 24 using binary search with inclusive bounds low=0, high=6:

| Step | low | high | mid=floor((low+high)/2) | value at mid | Decision |
|---:|---:|---:|---:|---:|---|
| 1 | 0 | 6 | 3 | 17 | target is larger; low=4 |
| 2 | 4 | 6 | 5 | 31 | target is smaller; high=4 |
| 3 | 4 | 4 | 4 | 24 | found |

Each comparison discards about half of the remaining range, so the number of comparisons grows logarithmically. Linear search works on unsorted lists but may inspect all n elements. Binary search on unsorted data is incorrect because “smaller” and “larger” do not tell us which half can be discarded.

### Selection-sort pass

Sort [5, 2, 4, 1]. Selection sort finds the smallest element in the unsorted suffix and swaps it into the next final position.

~~~text
start:       [5, 2, 4, 1]
pass 1:      [1, 2, 4, 5]  (minimum of all four placed at index 0)
pass 2:      [1, 2, 4, 5]  (minimum of suffix [2,4,5] already first)
pass 3:      [1, 2, 4, 5]
~~~

It performs Θ(n²) comparisons even when the list is already sorted. The method is easy to trace, but merge sort or a library sort is usually preferred for larger inputs.

## 6. Choose a data structure from the operation

| Need | Suitable structure | Why |
|---|---|---|
| Read item at known index | List/array | Direct indexing is typically O(1) |
| Add/remove newest item | Stack | LIFO order matches undo/call history |
| Serve requests in arrival order | Queue | FIFO order is fair and predictable |
| Test membership, remove duplicates | Set | Stores distinct values and supports membership |
| Look up value by identifier | Dictionary/map | Key maps to associated value |

The cost depends on implementation. In a dynamic array, append is usually amortized O(1), but insertion at index 0 shifts many values and is O(n). A hash-table dictionary has expected O(1) lookup under good hash distribution, but worst-case collisions can be worse. A balanced search tree supports ordered operations in O(log n); an ordinary unbalanced BST can degrade to O(n).

### Database example from requirements to query

Suppose the tables are:

~~~text
Student(student_id, name, class_id)
Marks(student_id, subject, score)
~~~

Question: show each student's name and score for Math marks of at least 80, ordered from highest score down.

~~~sql
SELECT s.name, m.score
FROM Student AS s
JOIN Marks AS m ON m.student_id = s.student_id
WHERE m.subject = 'Math' AND m.score >= 80
ORDER BY m.score DESC;
~~~

Read the query in stages: FROM/JOIN identify source rows; WHERE filters rows; SELECT chooses output columns; ORDER BY sorts the result. The primary key Student.student_id identifies each student. Marks.student_id is a foreign key, so a mark refers to an existing student.

To count students by class and retain only classes with at least 20 students:

~~~sql
SELECT class_id, COUNT(*) AS student_count
FROM Student
GROUP BY class_id
HAVING COUNT(*) >= 20;
~~~

WHERE filters individual rows before grouping. HAVING filters groups after aggregation. COUNT(*) counts rows; COUNT(score) ignores NULL scores. NULL means unknown or missing, so comparisons such as score = NULL do not work; SQL uses IS NULL.

**Questions with answers**

1. Which search is valid on an unsorted list? Linear search.
2. What is the time complexity of binary search comparisons? O(log n), when the data is sorted and middle access is efficient.
3. Why can a dictionary be better than a list for checking a student's ID? It uses an ID key and expected constant-time hash lookup, while scanning a list takes O(n).
4. Why use a foreign key? To enforce that a reference points to a row that exists.
5. Why is HAVING used for COUNT(*) >= 20? The condition applies to grouped results.
6. What does COUNT(score) do with NULL? It skips NULL values; COUNT(*) counts the row regardless.
