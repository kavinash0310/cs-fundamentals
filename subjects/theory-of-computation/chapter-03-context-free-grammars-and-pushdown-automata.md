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

## Full worked lesson: derive a string and track a PDA

Consider G with S→aSb | ε. Each recursive step adds one a on the left and one b on the right. Therefore L(G)={a^n b^n | n≥0}. A derivation of aabb is:
S ⇒ aSb ⇒ aaSbb ⇒ aabb.
The parse tree has one recursive layer per matched pair. The grammar encodes both the equal-count condition and the order condition; a regular expression cannot generally count an unbounded number of a's and then match the same number of b's.

### PDA intuition

A pushdown automaton has finite control plus an unbounded stack. For each input a, push a marker A. When b's begin, pop one A per b. Accept only when the input ends and the stack has returned to its bottom marker. The stack remembers the number of a's, something a finite automaton cannot do for arbitrary n.

~~~text
input a a a b b b
      push A A A | pop A A A
stack bottom -> A A A -> bottom
~~~

A PDA for this language must also reject strings such as aba: once the machine switches from reading a's to b's, a later a is invalid. Nondeterministic PDAs recognize exactly the context-free languages; deterministic PDAs recognize a proper subset, which includes many programming-language parsing needs but not every CFL.

### Ambiguity and grammar structure

A grammar is ambiguous if some string has two distinct parse trees (equivalently, two distinct leftmost derivations). The grammar E→E+E | E*E | id is ambiguous because id+id*id can group as (id+id)*id or id+(id*id). Separate nonterminals by precedence:
E→E+T | T; T→T*F | F; F→(E) | id.
Associativity is also encoded by recursive structure: E→E-T | T gives left association. An ambiguous grammar may describe a language correctly but still be unsuitable when a compiler needs one unambiguous parse.

### Normal forms and closure facts

Chomsky normal form uses productions A→BC or A→a (plus an optional start ε rule). Conversion removes ε-productions, unit productions, and useless symbols before restructuring rules. It is valuable for theory and the CYK algorithm, though hand-written programming-language parsers often use other forms.

CFLs are closed under union, concatenation, and Kleene star, but not generally under intersection or complement. A reliable non-CFL proof often uses closure: if L were context-free, then intersecting it with a suitable regular language would preserve context-freeness.

### Questions

1. What resource lets a PDA recognize equal numbers of a's then b's? Its stack.
2. Why is S→aSb|ε unambiguous? Each outermost a must pair with the final b; removing them gives a unique smaller derivation.
3. Is the intersection of two CFLs always a CFL? No.
