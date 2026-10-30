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

## Relational algebra and calculus: semantics

Relational algebra operators are closed over relations. Selection keeps tuples satisfying a predicate; projection removes attributes and, in pure relational algebra, duplicate tuples; join is product followed by matching selection. Union requires union-compatible schemas.

SQL often uses bag semantics: duplicate rows may remain unless DISTINCT is requested. Therefore a SQL query is not always identical to pure relational algebra without duplicate handling.

Tuple calculus expresses a declarative condition over tuple variables. Safe expressions range over database values and return finite results. Algebra and safe relational calculus have equivalent expressive power for standard relational queries, though SQL adds NULL, duplicates, ordering, and aggregates.

Key derivation example: R(A,B,C), F={A→B,B→C}. A⁺={A,B,C}, so A is a key. Any key must include A because no dependency produces A. Candidate key is {A}.

**Practice:** Translate “names of students with an enrollment in course X” into relational algebra and tuple calculus; identify whether projection may remove duplicates in relational algebra versus SQL.

## Extended lecture: design a relational schema from requirements

Start with entities, attributes, and relationships; then decide which facts need independent identity. A university example may have Student(StudentID, Name), Course(CourseID, Title), and Enrollment(StudentID, CourseID, Term, Grade). Enrollment is not merely a list inside Student: it represents a many-to-many relationship and has its own attributes. Its natural key may be (StudentID, CourseID, Term), while a surrogate EnrollmentID is optional if it does not replace the need to enforce the natural uniqueness rule.

### ER-to-relational mapping

- Strong entity: create a relation; choose a candidate key as primary key.
- Composite attribute: store its components (for example, address into street/city/postcode) when they need independent queries.
- Multivalued attribute: create a separate relation with owner key plus value.
- 1:N relationship: place the 1-side key as a foreign key on the N-side, unless relationship attributes or optionality suggest a separate table.
- M:N relationship: create a relation containing both entity keys and relationship attributes.
- Weak entity: include its identifying owner's key in its primary key.

The foreign-key direction is frequently tested. For Department 1—N Employee, DepartmentID belongs as a foreign key in Employee. This avoids duplicating a list of employee IDs in each department row.

### Relational algebra as query execution

Selection σ filters rows; projection π chooses columns; rename ρ changes relation/attribute names; union, difference, and Cartesian product combine compatible relations; join combines tuples by a condition. For “names of students enrolled in course CS101”:
1. Select Enrollment rows where CourseID=CS101.
2. Join them with Student on StudentID.
3. Project Name.
A relational algebra expression is a logical plan; an optimizer may reorder operations when relational equivalences preserve the result. Push selections toward the leaves to reduce intermediate tuples, but account for NULL and outer-join semantics in SQL.

### Integrity constraints

Primary key requires uniqueness and non-NULL values. Foreign key requires each non-NULL reference to match a referenced key (unless the declared action handles deletion/update differently). CHECK enforces a row predicate; NOT NULL rejects missing values. A schema is a set of allowed states, and constraints define which database states are legal.

### Practice

For M:N Author–Book with attribute RoyaltyPercent, create Author, Book, and AuthorBook(AuthorID, BookID, RoyaltyPercent). For a 1:N Course–Section relation, put CourseID in Section. Explain why an enrollment grade belongs to the relationship: it describes a student's participation in a particular course offering.
