# Chapter 2 — SQL Queries, Joins, and Constraints

## 1. Query structure
~~~sql
SELECT department, COUNT(*) AS n
FROM Student
WHERE active = TRUE
GROUP BY department
HAVING COUNT(*) >= 10
ORDER BY n DESC;
~~~
Logical order: FROM/JOIN, WHERE, GROUP BY, HAVING, SELECT, ORDER BY. WHERE filters rows before groups; HAVING filters groups.

## 2. Joins and NULL
INNER JOIN returns matching pairs. LEFT JOIN keeps all left rows and fills missing right columns with NULL. Use ON for join condition. NULL means unknown/missing, so compare with IS NULL, not = NULL. SQL uses three-valued logic; UNKNOWN is filtered out by WHERE unless expression evaluates TRUE.

Aggregate COUNT(*) counts rows; COUNT(column) skips NULL. Grouping each non-aggregated selected column is required in standard SQL.

## 3. Subqueries and set operations
EXISTS tests whether a correlated result has any row. IN tests membership but NULL may complicate NOT IN. UNION removes duplicates; UNION ALL keeps them. CTEs name subqueries for readability; recursive CTEs can express hierarchical traversal.

## 4. Constraints and transactions
PRIMARY KEY, UNIQUE, NOT NULL, CHECK, and FOREIGN KEY enforce rules near data. Transactions group changes; COMMIT persists, ROLLBACK undoes uncommitted changes.

## 5. Worked query
Find customers with at least two orders:
~~~sql
SELECT customer_id, COUNT(*) AS orders
FROM Orders
GROUP BY customer_id
HAVING COUNT(*) >= 2;
~~~
Filtering order dates uses WHERE before grouping. Filtering count uses HAVING.

## 6. Questions and practice
**Q1.** WHERE vs HAVING? **A:** Row filter vs group filter.
**Q2.** Count non-null email values? **A:** COUNT(email).
**Q3.** How test missing value? **A:** IS NULL.
Practice: write inner/left join queries; find duplicate keys; use EXISTS; explain why a left join can become inner if WHERE filters right-side NULLs.

## Notebook work
Write one query with join, aggregation, HAVING, and a NULL-safe condition.


## SQL NULL and join worked examples

Tables Student(sid,name) and Enrollment(sid,cid). To list every student and matching course:
~~~sql
SELECT s.name, e.cid
FROM Student AS s
LEFT JOIN Enrollment AS e ON e.sid=s.sid;
~~~
Students without a match appear with NULL cid. Adding WHERE e.cid='CS101' removes those NULL rows, effectively turning this result into an inner-match filter. Put the restriction in ON if unmatched students must remain:
~~~sql
LEFT JOIN Enrollment AS e
  ON e.sid=s.sid AND e.cid='CS101'
~~~

Three-valued logic: NULL='x' is UNKNOWN; WHERE retains only TRUE. NOT IN with a subquery containing NULL may evaluate UNKNOWN unexpectedly; NOT EXISTS is often safer.

Aggregation example: COUNT(*) counts each row; COUNT(score) skips null scores; AVG(score) divides by non-null count. GROUP BY department creates one aggregate row per group.

**Practice:** Write query for departments with average salary > 80,000; return authors even with no books; explain result of NULL=NULL and IS NULL.

## Join and grouping practice

Suppose Student(sid,name,dept), Enroll(sid,cid,grade), Course(cid,title). Query CS student names and course titles:
~~~sql
SELECT DISTINCT s.name, c.title
FROM Student s
JOIN Enroll e ON e.sid=s.sid
JOIN Course c ON c.cid=e.cid
WHERE s.dept='CS';
~~~
The join path follows foreign keys. DISTINCT is needed only if duplicate output pairs are possible and the desired result is a set.

To find departments with at least two students:
~~~sql
SELECT dept, COUNT(*) AS n
FROM Student
GROUP BY dept
HAVING COUNT(*) >= 2;
~~~
Do not put COUNT(*) in WHERE because aggregation occurs after row filtering.

A correlated EXISTS can test membership without multiplying outer rows. For “students with at least one enrollment,” EXISTS may express intent more directly than joining and deduplicating.

**Practice:** Use LEFT JOIN to retain students with no enrollment; write a NOT EXISTS query for courses with no students; show how a one-to-many join changes row count.