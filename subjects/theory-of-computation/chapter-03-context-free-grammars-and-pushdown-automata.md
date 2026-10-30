# Chapter 3 — Context-Free Grammars and Pushdown Automata

## 1. Grammars and derivations
A CFG is (V,Σ,R,S): variables, terminals, productions, and start variable. A derivation replaces one variable using a production. A parse tree records the derivation structure.

Example grammar S→aSb | ε generates {aⁿbⁿ:n≥0}. Derivation S⇒aSb⇒aaSbb⇒aabb.

Ambiguity occurs when a string has two distinct parse trees or leftmost derivations. Expression grammars use precedence/associativity rules to avoid ambiguity.

## 2. Normal forms
A grammar may be transformed to Chomsky normal form: productions A→BC or A→a, with optional start→ε. Removing ε-productions, unit productions, and useless variables must preserve the language, with care around ε.

## 3. Pushdown automata
A PDA is a finite automaton with a stack. A transition may inspect input and stack top, consume input, and push/pop symbols. The stack lets a PDA remember unbounded nested structure.

For aⁿbⁿ, push one marker per a, then pop one per b; accept when input ends and stack markers are gone. This cannot be done by finite memory alone.

## 4. CFG/PDA relationship
A language is context-free iff some PDA recognizes it. A CFG gives recursive nested structure; a PDA operationally stores pending matching obligations. A DPDA is deterministic and recognizes only a subset of CFLs.

## 5. Questions and practice
**Q1.** What language does S→aSb|ε generate? **A:** Equal counts of a's followed by b's.
**Q2.** What memory distinguishes a PDA from an FA? **A:** Unbounded stack.
**Q3.** Is every CFL regular? **A:** No; aⁿbⁿ is CFL but not regular.
**Q4.** What signals grammar ambiguity? **A:** Multiple parse trees for one string.
Practice: derive aabb; draw its parse tree; design a PDA for balanced parentheses; rewrite a simple grammar toward CNF.

## Notebook work
Draw the stack trace for input aaabbb and a parse tree for aabb.


## Trace a PDA and derivation

For S→aSb|ε, derive aabb:
~~~text
S => aSb => aaSbb => aaεbb => aabb
~~~
The parse tree pairs each recursive a with a later b. PDA pushes marker A for each a; on the first b it switches phase and pops one A per b. Reject any a after entering b-phase, extra b when stack is empty, or leftover A at end.

A PDA may accept by final state or empty stack; the conventions have equivalent language power, though a construction must match its acceptance rule. Stack top is the only directly accessible memory.

**Ambiguity check:** Grammar E→E+E|E*E|id can parse id+id*id with either root operator, so it is ambiguous. Splitting expression/term/factor layers encodes precedence.

**Practice:** For balanced parentheses, push on '(' and pop on ')'; reject underflow and require empty stack at end. Write a CFG for aⁿbⁿ and draw the parse tree for aabb. Identify leftmost and rightmost derivations.