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