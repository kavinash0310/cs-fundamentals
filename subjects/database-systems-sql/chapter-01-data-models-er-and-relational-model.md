# Chapter 1 — Data Models, ER Design, and Relational Model

## 1. Conceptual design
An entity is distinguishable object; an attribute describes it; a relationship connects entities. ER diagrams model entities, relationships, attributes, and cardinalities (1:1, 1:N, M:N). Participation may be total or partial.

Example: Student enrolls in Course is M:N and has attribute grade. Map it to Enrollment(StudentID,CourseID,grade) with a composite key.

## 2. Relational model
A relation is a set of tuples over attributes; tables approximate relations. A key uniquely identifies rows. Candidate keys are minimal superkeys; one is selected as primary. Foreign keys reference candidate/primary keys. Domain constraints define allowed values; entity integrity forbids null primary keys; referential integrity enforces valid references.

## 3. Relational algebra
Selection σ filters rows; projection π selects columns; rename ρ changes names; union/intersection/difference combine compatible relations; Cartesian product pairs rows; join combines matching rows.

Example: σ_dept='CS'(Student) filters CS students; π_name(...) returns names. Join Student and Enrollment on student ID to find course registrations.

Tuple relational calculus is declarative: specify which tuples satisfy a logical predicate, e.g. {s.name | Student(s) ∧ s.dept='CS'}. Relational algebra and safe relational calculus have the same expressive power for relational queries. SQL expresses similar logic and adds duplicate/null behavior.

## 4. Questions and practice
**Q1.** Where does an M:N relationship go in relational design? **A:** Separate relation with foreign keys and relationship attributes.
**Q2.** Candidate key vs superkey? **A:** Candidate is minimal.
**Q3.** What does projection do? **A:** Selects attributes/columns.
Practice: model a library; identify keys; translate a sentence into σ and π; distinguish entity from relationship.

## Notebook work
Draw an ER diagram and map each entity/relationship to relations with keys.


## Relational calculus worked translation

Tuple relational calculus uses tuple variables and predicates. Query CS student names:
~~~text
{ s.name | Student(s) AND s.dept = 'CS' }
~~~
Find students enrolled in a course:
~~~text
{ s.name | Student(s) AND EXISTS e (Enrollment(e) AND e.sid=s.sid) }
~~~
This describes which tuples satisfy a condition, not an execution plan. Safe calculus restricts results to values from the database domain so queries return finite relations.

Candidate key example: relation R(A,B,C), dependencies A→B and B→C. A⁺={A,B,C}, so A is a candidate key. B⁺={B,C}; C⁺={C}. If a key is composite, test each proper subset for minimality.

ER mapping reminder: 1:N puts the one-side key as foreign key on the many-side; M:N creates a relation containing both keys and relationship attributes; weak entity key includes owner key plus partial key.

**Practice:** Translate “employees in department D with salary > x” into calculus; compute closures of A and B for the dependencies; identify candidate key and foreign-key direction.