# Chapter 2 — Inheritance, Polymorphism, and Interfaces

## 1. Inheritance
Inheritance expresses a subtype relationship only when the subtype can safely stand in for the base type. Reuse alone is not sufficient reason. A derived class inherits/extends behavior and may override methods; constructors establish base and derived invariants.

The Liskov substitution principle: code expecting base type should remain correct when given subtype. A Square that changes width/height independently may violate a Rectangle contract that assumes independent setters.

## 2. Polymorphism
Overloading selects among same-name methods using compile-time signature. Overriding replaces virtual behavior selected by runtime type (language-dependent dispatch). Static type controls accessible interface; dynamic type may determine implementation.

Interfaces define capabilities without requiring shared representation. Composition delegates behavior to contained objects and often avoids fragile inheritance hierarchies.

## 3. Abstract classes and contracts
Abstract classes may share partial implementation and state; interfaces often specify a contract. Preconditions, postconditions, and invariants make behavior testable.

## 4. Questions and practice
**Q1.** Overloading vs overriding? **A:** Signature-based compile-time choice vs subtype runtime behavior.
**Q2.** What does Liskov substitution protect? **A:** Correctness when a subtype replaces its base.
**Q3.** When prefer composition? **A:** When a has-a relationship or replaceable behavior fits better than is-a.
Practice: model payment methods through an interface; find a contract violation; trace an overridden call.

## Notebook work
Draw a class/interface diagram and annotate which method dispatches at runtime.


## Polymorphism trace and substitution test

Suppose Shape has virtual area(); Circle and Rectangle override it. A function printArea(Shape s) calls s.area(). Runtime dispatch selects the override based on actual object type, while the static type ensures area() is in the available contract.

A subtype must preserve expectations. If a base method accepts any positive width and promises width can later be changed independently of height, a Square subtype that forces both equal violates that contract. Prefer an immutable Rectangle or a separate Quadrilateral abstraction if both concepts must be modeled.

Composition example: a Car has an Engine interface. ElectricEngine and CombustionEngine can be supplied; the Car need not inherit from an engine class.

**Practice:** Trace overridden method through base reference; identify compile-time overload choice; state pre/postconditions for a payment interface.