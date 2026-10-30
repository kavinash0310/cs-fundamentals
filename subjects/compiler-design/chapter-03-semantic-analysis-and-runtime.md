# Chapter 3 — Semantic Analysis, Translation, and Runtime

## 1. Semantic checks
Parsing checks grammatical structure; semantic analysis checks meaning: declaration before use, type compatibility, function arguments, scope, and return paths. A symbol table maps names to type, scope, storage, and other attributes.

Static type checking rejects invalid operations before execution. Implicit conversions need defined rules; coercion can introduce precision loss.

## 2. Syntax-directed translation
Attributes attach values or code to grammar nodes. Synthesized attributes flow upward (e.g., expression type); inherited attributes flow from parent/context (e.g., expected type). Syntax-directed definitions associate semantic rules with productions.

Three-address code uses simple instructions such as t1=a*b; t2=t1+c. It separates source syntax from target machine details.

## 3. Runtime environments
A function call creates an activation record containing parameters, return address, saved registers, locals, and temporaries. Stack allocation works for nested calls with last-in-first-out lifetimes. Heap allocation supports objects whose lifetimes outlast a call.

~~~text
stack top -> [callee locals/temps]
             [saved return + caller frame]
~~~

Closures and recursion require preserving the correct environment. Stack overflow occurs when call depth or local allocation exceeds available stack.

## 4. Questions and practice
**Q1.** Is undeclared-variable detection parsing? **A:** Semantic analysis.
**Q2.** Synthesized attribute direction? **A:** Child to parent.
**Q3.** What is stored in an activation record? **A:** Call-specific state such as return address and locals.
Practice: annotate expression types; produce three-address code for a*b+c; draw frames for recursive factorial.

## Notebook work
Trace one function call and return, labeling the stack frame fields.


## Typed translation and runtime frame example

For x = a + b*c, a simple translation is:
~~~text
t1 = b * c
t2 = a + t1
x = t2
~~~
Type checking first verifies each operator accepts its operands and that the result can be assigned to x. A syntax-directed rule may synthesize expression type upward and generate code after child expressions.

A typical activation record contains arguments/parameters, return address, saved registers, local variables, and temporaries. Recursive calls allocate distinct frames. A frame pointer gives stable offsets even when stack pointer moves during temporary pushes.

Static scoping resolves names by lexical nesting; dynamic scoping follows call history. Most mainstream compiled languages use static/lexical scope, but closure conversion may preserve captured variables in an environment object.

**Practice:** Draw two recursive factorial frames; identify where a captured variable lives; type-check a+b when a is integer and b is string under a language with no implicit conversion.