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

## 5. The object model in detail

An object combines identity, state, and behavior:
- **Identity** distinguishes this object from another, even when their fields match.
- **State** is the current value of its fields.
- **Behavior** is the set of operations it supports.

Two Student objects can both have name="Asha" and roll=17. If they represent separate enrollments, identity may differ; if they represent the same persistent student, domain equality may be based on roll number. Decide from the problem domain.

A class is a type definition. It describes what fields and operations an instance has, but many languages also allow class-level state/operations. Instance fields belong to each object; static/class fields are shared by the class.

~~~text
class definition                     two instances
+-----------------------+             +-------------+
| Student               |             | id: 17      |
| id                    |   create    | name: Asha  |
| name                  | ----------> +-------------+
| changeName()          |
+-----------------------+             +-------------+
                              create  | id: 18      |
                                      | name: Ravi  |
                                      +-------------+
~~~

**Encapsulation** is not merely hiding fields. It means the object controls changes so its invariants remain true. If balance must never be negative, exposing a public writable balance field breaks the invariant. A withdraw operation can validate the amount and perform the update atomically within the object's contract.

### Constructors and invariants

A constructor establishes a valid initial state. A constructor that accepts null for a required name or a negative quantity may create an invalid object. Options include rejecting invalid arguments, using a factory that returns a validated object, or representing “not yet configured” as a separate type/state.

An invariant is a condition that must hold before and after every public operation. Example: for a date, 1≤day≤daysInMonth(year,month). If every method must repair invalid state after mutation, the boundary is poorly designed.

### Method contracts

Document each operation with:
- Preconditions: what the caller must provide.
- Postconditions: what is true after success.
- Invariants: what remains true across operations.
- Failure behavior: exception, error result, or no change.

Deposit(amount) precondition amount>0; postcondition balance increases exactly by amount. Withdraw(amount) requires amount>0 and amount≤balance; after success balance decreases by amount. Decide whether invalid operations throw, return a result, or are rejected by a domain service.

### Mutability, aliasing, and defensive copies

If two variables point to the same mutable object, a change through one alias is visible through the other:

~~~text
a ----+
      +----> [list: 2, 4]
b ----+
a.append(6) changes what b observes too
~~~

A shallow copy duplicates the outer object but may retain references to nested mutable values. An immutable value object cannot be changed after construction; operations return a new value. Immutability simplifies reasoning, safe sharing, and hashing.

Do not return a mutable internal collection directly when callers could violate invariants. Return an immutable view or defensive copy, or expose domain operations such as addItem/removeItem.

### Equality and hashing

Value equality answers whether two values represent the same domain value. Identity equality answers whether they are the same object. Hash maps rely on a contract: if a.equals(b), then hash(a)=hash(b). Unequal objects may share a hash; collisions are allowed.

Never mutate fields participating in a key's hash while the object is stored in a hash map. The object may end up in a bucket selected by its old hash and become difficult to find.

### Language notes

Java distinguishes references and primitive values; equals/hashCode are commonly overridden for value equality. C++ copy/move constructors and destructors affect ownership; raw pointers need explicit lifetime discipline, while smart pointers express ownership. Python names bind to objects; == commonly compares values while is checks identity. These syntax details differ, but invariants and ownership questions apply across languages.

### Worked design: Temperature

Store a temperature as integer milli-degrees Celsius to avoid accidental floating equality. Constructor accepts the supported range. toFahrenheit computes a display value without changing stored state. Equality compares canonical stored representation. A display string is not the source of truth.

### Questions and answers

**Q1.** Can two objects be equal but not identical?  
**Answer:** Yes. Separate value objects may contain equal values while occupying distinct identities.

**Q2.** Why is returning a private mutable list unsafe?  
**Answer:** A caller can mutate it without using the object's validation rules.

**Q3.** Where should a validity rule live?  
**Answer:** At the boundary that owns the state, or in a shared value type/domain rule used by that boundary.

**Q4.** Why must equal hash keys have equal hashes?  
**Answer:** Hash lookup first chooses a bucket from the hash; inconsistent hashes can make equal keys unreachable.

**Q5.** Is a constructor only a place to copy arguments into fields?  
**Answer:** No. It establishes the object's initial invariant and should reject invalid state.

### Practice with answer guide

1. Design a Money value with amount and currency. Which equality rule is appropriate?  
   **Guide:** Compare both amount and currency; define precision/rounding policy.
2. A caller can modify a Course's internal student list. Repair the API.  
   **Guide:** Return an immutable/copy view and provide validated enrollment methods.
3. A hash-map key's name changes after insertion. Explain the failure.  
   **Guide:** Its hash bucket may no longer match where it was stored.
4. Give a constructor precondition for a positive-dimension Rectangle.  
   **Answer:** width>0 and height>0.
5. Draw the state transition of an order: Draft→Submitted→Paid→Shipped→Delivered. Which transitions should be rejected?

## Notebook diagram

Draw the class boundary, invariant, public operations, and two object instances. Then sketch two aliases pointing at one mutable list and explain the risk.
