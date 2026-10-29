# Chapter 1 — Logic, Sets, Relations, and Proof

## Learning goals

Translate statements into logic, use quantifiers and set operations precisely, test whether a relation is an equivalence or partial order, and build short proofs.

## 1. Propositions and connectives

A proposition is a statement that is true or false. For propositions p and q:

| Symbol | Meaning | False/true condition |
|---|---|---|
| ¬p | not p | reverses truth |
| p ∧ q | p and q | true only if both true |
| p ∨ q | p or q | false only if both false |
| p → q | if p then q | false only when p true and q false |
| p ↔ q | p iff q | true when both have same truth value |

The implication p→q is equivalent to ¬p∨q. Its contrapositive ¬q→¬p is equivalent to the original; the converse q→p is not generally equivalent.

Example: “If a number is divisible by 4, it is even.” The converse “if even, then divisible by 4” is false (6 is even). The contrapositive says “if odd, not divisible by 4,” which is true.

## 2. Predicates and quantifiers

A predicate P(x) becomes a proposition after x is assigned or quantified. Universal quantification ∀x P(x) means every x in the domain satisfies P. Existential quantification ∃x P(x) means at least one does.

Negation rules:
- ¬(∀x P(x)) ≡ ∃x ¬P(x)
- ¬(∃x P(x)) ≡ ∀x ¬P(x)

To disprove a universal statement, one counterexample suffices. To prove an existential statement, exhibit a witness.

Example: “Every integer n has an integer m with m>n” is ∀n∈Z ∃m∈Z(m>n), true by choosing m=n+1. Reversing quantifiers changes meaning: ∃m ∀n(m>n) is false.

## 3. Sets, functions, and cardinality

Set operations include union A∪B, intersection A∩B, difference A\\B, and complement Aᶜ relative to a universe U. De Morgan laws: (A∪B)ᶜ=Aᶜ∩Bᶜ and (A∩B)ᶜ=Aᶜ∪Bᶜ.

A function f:A→B assigns exactly one output in B to each input in A. It is injective if equal outputs imply equal inputs; surjective if every element of B is reached; bijective if both. A bijection pairs the sets one-to-one and proves equal cardinality.

Composition: if f:A→B and g:B→C, then (g∘f)(x)=g(f(x)). Function composition is associative but usually not commutative.

## 4. Relations

A binary relation R on A is a subset of A×A. Write aRb when (a,b)∈R.

- Reflexive: aRa for every a.
- Symmetric: aRb implies bRa.
- Antisymmetric: aRb and bRa imply a=b.
- Transitive: aRb and bRc imply aRc.

An equivalence relation is reflexive, symmetric, and transitive. It partitions A into equivalence classes. Congruence modulo n is an example: a≡b (mod n) iff n divides a-b.

A partial order is reflexive, antisymmetric, and transitive. Divisibility on positive integers is a partial order: 2|6 and 6|2 cannot both hold unless equal; many pairs are incomparable. Subset inclusion is another partial order.

## 5. Proof techniques

**Direct proof:** assume hypotheses and derive the conclusion. To prove the sum of two even integers is even, write them as 2a and 2b; their sum is 2(a+b).

**Contrapositive:** prove ¬Q→¬P instead of P→Q. To prove n² odd implies n odd, prove even n gives even n².

**Contradiction:** assume the conclusion is false and derive an impossibility. Use sparingly and identify the contradiction clearly.

**Induction:** prove base case P(0) or P(1); assume P(k); prove P(k+1). Strong induction assumes all earlier cases through k. Use induction for sums, divisibility, and recursively defined objects.

Example: 1+2+...+n=n(n+1)/2. Base n=1 holds. If it holds for n, adding n+1 gives n(n+1)/2+n+1=(n+1)(n+2)/2.

## 6. Worked checks

1. Negate “Every student solved some problem.” Answer: There exists a student who solved no problem.
2. Is ≤ on integers an equivalence? No: it is not symmetric. It is a partial order because it is reflexive, antisymmetric, transitive.
3. Is “same remainder modulo 5” an equivalence? Yes: reflexive, symmetric, transitive.
4. Is f(x)=x² from integers to integers injective? No: f(2)=f(-2).

## 7. Questions and practice

**Q1.** When is p→q false? **A:** p true and q false.
**Q2.** What is the negation of ∃x∀y P(x,y)? **A:** ∀x∃y¬P(x,y).
**Q3.** Which properties define an equivalence relation? **A:** Reflexivity, symmetry, transitivity.
**Q4.** Give a counterexample to “If a product ab is even, both a and b are even.” **A:** a=2,b=3.
**Q5.** What property makes a function a bijection? **A:** Both injective and surjective.
**Practice:** Prove the sum of two odd integers is even; test divisibility on positive integers for each relation property; write the converse and contrapositive of a conditional statement; prove by induction that 1+3+...+(2n-1)=n².

## Notebook work

Draw a truth table for implication, a Venn diagram for De Morgan's law, and a relation-property grid for divisibility and equality.
