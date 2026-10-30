# Chapter 1 — Objects, Classes, and Encapsulation

## 1. Classes and objects
A class defines state and behavior; an object is an instance with identity. Fields hold state, methods define operations, constructors establish valid initial state. An object should own invariants and protect them from invalid external mutation.

Example BankAccount: private balance, deposit(amount>0), withdraw(amount>0 and <=balance). Exposing balance for arbitrary writes would bypass rules.

## 2. Encapsulation and abstraction
Encapsulation groups state/behavior and controls access. Abstraction exposes essential operations while hiding implementation. A client should depend on an interface such as withdraw rather than storage details.

Access control (public/protected/private) differs by language, but the design goal is stable boundaries, not simply making all fields private.

## 3. Identity, equality, and lifecycle
Two objects can have equal values but different identities. Define equality by domain meaning; hash-based collections require equal objects to have equal hashes. Resource lifecycle (files, sockets, locks) needs deterministic cleanup or scoped management.

## 4. Questions and practice
**Q1.** Class vs object? **A:** Type/blueprint vs instance.
**Q2.** Why encapsulate balance? **A:** Preserve account invariants.
**Q3.** Must value-equal objects share a hash? **A:** Yes, in hash collections.
Practice: model Student and Course; define invariants and public methods; identify identity versus value equality.

## Notebook work
Draw an object with private fields, public operations, and an invariant.


## Invariant and lifecycle example

Design a bounded BankAccount with balance≥0:
- Constructor rejects a negative opening balance.
- deposit requires amount>0 and adds it.
- withdraw requires amount>0 and amount≤balance.
- balance is observable but not arbitrarily writable.
The invariant is checked before and after every public operation.

Value equality example: Money(5,"USD") equals another Money(5,"USD"), but not Money(5,"EUR"). If used in a hash map, equals and hashCode must agree. Mutable keys are dangerous because changing hash-relevant fields can make entries unreachable.

Resource ownership should be explicit: who closes a file/socket? Prefer scoped cleanup patterns (RAII, try-with-resources, context managers) in the language used.

**Practice:** Define invariants for ShoppingCart and CourseEnrollment. Identify which fields should be immutable and what happens if duplicate identifiers are inserted.