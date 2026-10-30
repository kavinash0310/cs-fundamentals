# Chapter 2 — Grammars, Parsing, and Syntax Trees

## 1. Context-free syntax
A grammar describes legal phrase structure. For arithmetic:
~~~text
E -> E + T | T
T -> T * F | F
F -> (E) | id
~~~
The grammar encodes multiplication precedence over addition; left recursion makes operators left-associative.

## 2. Top-down parsing
Recursive descent expands from start symbol toward input. LL(1) parsing uses one lookahead token and cannot handle left recursion directly. Remove left recursion and left-factor common prefixes.

For E→E+T|T, rewrite E→T E'; E'→+ T E'|ε. FIRST sets describe possible first terminals; FOLLOW sets describe what may follow a nonterminal. Predictive table conflicts indicate grammar is not LL(1) in that form.

## 3. Bottom-up parsing
Shift-reduce parsing shifts input onto a stack and reduces handles to nonterminals. LR parsers handle a larger grammar class. Conflicts may be shift/reduce or reduce/reduce and reflect ambiguity or inadequate grammar/table.

## 4. Parse trees and ASTs
Parse tree includes grammar detail; abstract syntax tree (AST) keeps essential structure. For a+b*c, AST root is +, left child a, right child * with b,c.

## 5. Questions and practice
**Q1.** What does FIRST(X) contain? **A:** Terminals that may begin strings derived from X (and ε when possible).
**Q2.** Why eliminate left recursion for recursive descent? **A:** It would recurse without consuming input.
**Q3.** Which is generally more powerful: LL(1) or LR? **A:** LR parsing handles a broader deterministic grammar class.
Practice: derive an expression; compute FIRST/FOLLOW for a small grammar; identify a parse conflict; draw AST for (a+b)*c.

## Notebook work
Show a leftmost derivation, parse tree, and simplified AST for one expression.


## FIRST/FOLLOW and predictive parsing example

Grammar:
~~~text
S -> a A
A -> b | ε
~~~
FIRST(A)={b,ε}. FIRST(S)={a}. FOLLOW(S)={$}; since A ends S's production, FOLLOW(A)={$}. Predictive table places A→b under lookahead b and A→ε under lookahead $. No cell has two productions, so this small grammar is LL(1).

For E→E+T|T, left recursion causes recursive descent to call E without consuming input. Rewrite using E→T E' and E'→+ T E'|ε. When factoring a grammar, preserve the same language and precedence/associativity.

Shift-reduce parsers maintain a stack of grammar symbols/states. A shift consumes input; a reduce replaces a handle by its left-hand nonterminal. A conflict is not automatically proof the language has no parser; a different grammar/parser class may resolve it.

**Practice:** Compute FIRST/FOLLOW for A→aB|ε, B→b; parse a+b*c with precedence grammar; identify whether subtraction is left-associative.