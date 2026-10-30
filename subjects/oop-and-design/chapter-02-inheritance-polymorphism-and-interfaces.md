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

## 5. Inheritance in detail

Inheritance creates a subtype relation: an object of derived type can be used where a base type is expected, subject to the base contract. Public inheritance in C++ and extends in Java are usually intended to model “is-a,” not simply code reuse. Private implementation inheritance and language-specific mechanisms have different semantics.

Single inheritance gives one direct base; multiple inheritance gives several and introduces ambiguity when bases contain the same member or a shared ancestor (the diamond problem). Languages resolve this with explicit qualification, virtual inheritance, interface rules, or other mechanisms. Prefer composition when the relation is “has-a.”

~~~text
             Shape
            /     \
         Circle  Rectangle

Circle IS-A Shape
Car HAS-A Engine  (composition)
~~~

### Subtyping and behavioral contracts

Liskov Substitution Principle means a caller relying on the base contract must still work with a subtype. A subtype must not strengthen preconditions or weaken postconditions. It may strengthen guarantees but must preserve promised behavior.

Suppose a base Account permits withdrawing any amount up to balance. A FixedDepositAccount override that rejects all withdrawals until maturity strengthens the precondition and may violate code expecting the base behavior. Model restrictions in an explicit interface or separate contract.

### Overloading, overriding, and hiding

| Mechanism | Selection | Key condition |
|---|---|---|
| Overloading | Usually compile-time | Same name, different parameter signatures |
| Overriding | Runtime dispatch for virtual methods | Subtype provides implementation matching base contract |
| Hiding | Name lookup rules | Subclass declaration may mask base member; behavior is language-specific |

Overload resolution uses static types and conversions. Runtime overriding uses the dynamic type of the receiver. Return type alone generally cannot distinguish overloads. Static methods are commonly hidden rather than dynamically overridden.

### Abstract class and interface

An abstract class can hold shared state and partial implementation. An interface specifies a capability/contract and may allow default behavior depending on language. A class can often implement multiple interfaces even when it can extend only one class.

Program to the needed abstraction: a report service may depend on a Storage interface, allowing file, database, or in-memory implementations. Avoid a giant interface that forces unrelated classes to implement meaningless methods.

### Worked dispatch example

~~~text
Base reference = new Circle()
reference.draw()
~~~

The compiler checks that draw is allowed by Base's declared interface. If draw is virtual/overridden, runtime dispatch invokes Circle.draw. If method is overloaded, the compile-time argument types select the overload first; runtime dispatch may then select the override of that signature.

### Exceptions and contracts

An override should preserve the base method's failure contract. A subtype that unexpectedly throws a new unchecked failure can surprise callers. Some languages constrain checked exceptions in overrides. Document which errors are possible and whether the operation leaves state unchanged after failure.

### Common mistakes

- Using inheritance solely to reuse code.
- Confusing overload resolution with runtime dispatch.
- Making fields protected and exposing subclass mutation that breaks invariants.
- Forgetting that constructors do not dispatch like ordinary virtual methods safely.
- Treating every interface as a marker with no behavior contract.
- Violating substitutability by strengthening preconditions.

### Questions and answers

**Q1.** What selects an overridden instance method?  
**Answer:** Runtime object type, when the method participates in dynamic dispatch.

**Q2.** What selects an overloaded method?  
**Answer:** Compile-time argument types and overload-resolution rules.

**Q3.** Why can Square be a problematic subtype of a mutable Rectangle?  
**Answer:** Independent width/height setters may contradict the square invariant or the rectangle's promised behavior.

**Q4.** Is implementing an interface inheritance?  
**Answer:** It is subtype conformance to a contract; it usually does not imply shared object representation.

**Q5.** When is composition preferable?  
**Answer:** When an object uses another capability, when implementation should be replaceable, or when inheritance would violate the subtype contract.

### Practice

1. Give a base-class contract for a read-only collection and a subtype that preserves it.
2. Trace overload choice for f(Object) and f(String) when passed a String variable and then an Object variable.
3. Refactor a class hierarchy where every subclass overrides a method to throw “unsupported.”
4. Explain why a Car should usually contain an Engine instead of inherit from Engine.
5. Draw a diamond inheritance graph and list one ambiguity that can occur.

**Answer guide:** Prefer the most specific overload allowed by the compile-time type; use capability-specific interfaces; favor composition when no genuine substitutable “is-a” relation exists.
