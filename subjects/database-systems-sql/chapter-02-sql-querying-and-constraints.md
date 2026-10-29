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
