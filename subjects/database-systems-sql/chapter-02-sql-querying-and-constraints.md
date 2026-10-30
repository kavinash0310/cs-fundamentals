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

## Extended lecture: reason about SQL result sets

SQL is declarative: write the desired relation and let the optimizer choose a physical plan. A useful conceptual order is FROM/JOIN, WHERE, GROUP BY, HAVING, SELECT, DISTINCT, ORDER BY, LIMIT. This explains why a SELECT alias is not generally available in WHERE: WHERE is conceptually evaluated earlier.

Suppose Student has rows (1,Ana), (2,Bo), (3,NULL). COUNT(*) counts all three rows; COUNT(Name) counts only non-NULL names, so it returns 2. COUNT(DISTINCT Name) counts distinct non-NULL values. NULL means unknown/missing, not zero and not an ordinary comparable value. Therefore Name = NULL is not true; use IS NULL. Comparisons with NULL yield UNKNOWN, and WHERE retains only rows whose condition is TRUE.

### LEFT JOIN: preserve unmatched left rows

~~~sql
SELECT s.id, e.course_id
FROM Student s LEFT JOIN Enrollment e
  ON e.student_id = s.id
 AND e.term = 'Fall';
~~~

This keeps every Student. Students with no Fall enrollment receive NULL in the e columns. Moving e.term='Fall' into WHERE filters those NULL-extended rows and can turn the query into an effective inner join. Put a condition in ON when it controls matching; put it in WHERE when it should filter the final joined result.

### Grouping example

To find departments with at least 10 employees:

~~~sql
SELECT department_id, COUNT(*) AS n
FROM Employee
GROUP BY department_id
HAVING COUNT(*) >= 10;
~~~

WHERE filters individual rows before grouping; HAVING filters groups after aggregation. Selecting a nonaggregated column that is not grouped is invalid or dialect-dependent because a group has no unique value for it.

### Constraints and safe updates

A UNIQUE constraint can permit multiple NULLs depending on the DBMS and constraint semantics. A transaction can enforce multi-row invariants that a row-level CHECK cannot. UPDATE or DELETE without a selective WHERE may affect every row; inspect the predicate and expected row count. Parameterized queries keep data separate from SQL syntax and prevent injection; string concatenation is unsafe even when inputs “look numeric.”

### Questions with answers

1. What does COUNT(column) skip? NULL values.
2. Why can LEFT JOIN plus right-table condition in WHERE lose rows? The WHERE predicate rejects NULL-extended rows.
3. Can SQL produce duplicates? Yes, relations in SQL are generally bags unless DISTINCT is requested.
4. How do you test for missing values? IS NULL or IS NOT NULL.
