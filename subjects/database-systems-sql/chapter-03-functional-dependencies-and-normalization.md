# Chapter 3 — Functional Dependencies and Normalization

## 1. Functional dependencies
X→Y means any two tuples agreeing on attributes X must agree on Y. A key functionally determines all attributes. Attribute closure X⁺ is all attributes derivable from X under dependencies; if X⁺ contains all relation attributes, X is a superkey.

Armstrong axioms: reflexivity, augmentation, transitivity. They support deriving dependencies. Minimal cover removes extraneous attributes and redundant dependencies.

## 2. Anomalies
Storing student, course, and instructor data in one table can cause:
- Update anomaly: instructor change repeated in many rows.
- Insertion anomaly: cannot record course before enrollment.
- Deletion anomaly: removing final student erases course data.

Normalization decomposes relations to reduce redundancy while preserving information.

## 3. Normal forms
1NF: atomic values, no repeating groups.
2NF: 1NF and no partial dependency of a non-prime attribute on part of a candidate key.
3NF: for every nontrivial X→A, X is a superkey or A is prime.
BCNF: for every nontrivial X→Y, X is a superkey.

BCNF is stricter than 3NF. A decomposition should ideally be lossless; dependency preservation is also desirable.

## 4. Worked decomposition
Relation R(Student,Course,Instructor) with key (Student,Course) and Course→Instructor. Instructor depends on part of composite key, violating 2NF. Decompose Course(Course,Instructor) and Enrollment(Student,Course). The join on Course reconstructs the original without spurious combinations under the stated dependency.

## 5. Questions and practice
**Q1.** What does X→Y assert? **A:** X uniquely determines Y values.
**Q2.** Which normal form removes partial dependency? **A:** 2NF.
**Q3.** Is BCNF stricter than 3NF? **A:** Yes.
Practice: compute closure; find candidate keys; identify anomalies; test lossless join on a decomposition.

## Notebook work
Write a dependency diagram and mark candidate keys, prime attributes, and the violating dependency.


## Attribute closure and decomposition worked steps

For F={A→BC, C→D, D→A}, compute A⁺: start {A}; A→BC adds B,C; C→D adds D; closure is {A,B,C,D}. Thus A is a superkey. To see if it is a candidate key, test minimality: empty set is not a key, so A is minimal.

A decomposition is lossless if joining projected relations cannot create spurious tuples. For binary decomposition R→R1,R2, a standard test under FDs is that common attributes functionally determine all attributes of at least one side.

3NF synthesis uses a minimal cover and creates a relation per dependency; add a key relation if none contains a candidate key. BCNF decomposition repeatedly splits on a violating FD, ensuring the determinant becomes a key in each resulting relation but may lose dependency preservation.

**Practice:** For R(A,B,C,D), F={A→B,B→C,C→D}, find candidate key A; identify BCNF violations if key is A; normalize stepwise and state lossless/dependency-preservation considerations.

## Normalization reasoning and lossless joins

For R(Student,Course,Instructor,Room) with FDs (Student,Course)→Instructor, Course→Room, candidate key is (Student,Course) if no other dependencies determine Student. Course→Room violates 2NF because Course is a proper part of the key and Room is non-prime. Decompose Course(Course,Room) and Enrollment(Student,Course,Instructor).

A lossless decomposition preserves the original relation under join, without spurious combinations. Dependency preservation means constraints can be checked on decomposed relations without joining them. Sometimes BCNF improves redundancy but loses dependency preservation; 3NF synthesis often preserves dependencies.

Armstrong derivation: from A→B, augmentation gives AC→BC; decomposition yields AC→B. Use closure rather than guessing keys: a key's closure must include every attribute and the key must be minimal.

**Practice:** For R(A,B,C) with A→B and B→C, find key and highest normal form; decompose into 3NF and test whether joins can create spurious tuples under the stated FDs.

## Extended lecture: find keys and decompose safely

A functional dependency X→Y means that whenever two tuples agree on attributes X, they must agree on Y. It is a statement about all legal database states, not merely the rows currently stored. Attribute closure X+ is computed by repeatedly applying dependencies whose left side is already contained in the closure.

**Worked closure.** Let R(A,B,C,D) and F={A→B, B→C, AC→D}. Compute A+: start {A}; A→B adds B; B→C adds C; now AC→D applies, so A+={A,B,C,D}. Thus A is a superkey. Since A is a single attribute, it has no proper nonempty subset, so it is a candidate key.

To find all candidate keys, do not stop at one key. Attributes that never occur on any FD right side must appear in every key. Test combinations in increasing size, compute closure, and discard any set that contains an already discovered key.

### 3NF and BCNF

A relation is in BCNF when every nontrivial FD X→A has X as a superkey. It is in 3NF when, for every nontrivial FD, X is a superkey or A is a prime attribute (part of some candidate key). BCNF is stricter and can sacrifice dependency preservation; 3NF synthesis aims to preserve dependencies while making a lossless decomposition.

Example R(A,B,C), F={A→B, B→C}. A is the candidate key because A+={A,B,C}. B→C violates BCNF: B is not a superkey. Decompose into R1(B,C) and R2(A,B). Their common attribute is B, and B→BC makes B a key of R1, so the decomposition is lossless. The dependencies A→B and B→C are each enforceable in one component, so they are preserved.

### Lossless-join test

For a binary decomposition R into R1 and R2, it is lossless with respect to F if the common attributes functionally determine all attributes of at least one component: (R1∩R2)→R1 or (R1∩R2)→R2 in F+. A lossy decomposition can create spurious tuples when the relations are joined back.

### Exam checklist

- State the candidate keys before identifying prime attributes.
- Use F+ or closure, not intuition about attribute names.
- Lossless join and dependency preservation are different properties.
- Decomposition removes redundancy/anomalies; it does not mean “split every table as far as possible.”
