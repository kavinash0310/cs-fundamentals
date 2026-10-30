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

## Worked notebook: scopes, types, and storage

Parsing establishes grammatical structure; semantic analysis asks whether that structure makes sense under the language rules. For example, x + true may be syntactically valid if + accepts two expressions, yet fail type checking because the operands have incompatible types. A symbol-table entry typically records a name, kind (variable/function/type), declared type, scope, storage class, and source location.

### Scope lookup by hand

~~~text
global: rate : real
function f:
    local rate : integer
    local tax  : real
    return rate + tax
~~~

When analyzing rate + tax inside f, lookup begins in the innermost scope. rate resolves to the local integer and hides the global real; tax resolves to the local real. If + permits integer-to-real promotion, the checker inserts or records a conversion. If it does not, compilation reports a type error. This shows why a symbol table is not simply a dictionary from name to one value: nested scopes can contain the same spelling.

### Runtime diagram: one function call

~~~text
higher addresses
+--------------------------+
| caller's older frame     |
+--------------------------+
| return address           |
| saved registers          |   activation record for f
| parameters               |
| local variables          |
| temporaries / spill slots|
+--------------------------+
| next call's frame        |
+--------------------------+
lower addresses / stack growth (architecture-dependent)
~~~

An activation record (stack frame) is created for a call and reclaimed when it returns. Exact ordering and stack-growth direction are ABI-dependent; the conceptual pieces matter more than a particular machine layout. A recursive function gets a distinct frame per active call, so each call has its own parameter/local values. Heap objects can outlive a call when references escape, which is why a returned pointer to a dead stack local is invalid in C.

### Call trace: recursive factorial

For fact(3), calls descend as fact(3) -> fact(2) -> fact(1) -> fact(0). Each frame stores its own n. At the base case, fact(0)=1; returns unwind as fact(1)=1, fact(2)=2, fact(3)=6. The call stack is last-in, first-out. A compiler may replace some tail calls with jumps, but ordinary factorial multiplication occurs after the recursive return, so this call is not tail-recursive.

### Semantic checks to perform

- Every identifier is declared before use, according to the language's scope rules.
- A function call has a known callee and compatible argument count/types.
- Assignment respects mutability and type compatibility.
- break, return, and continue occur in legal contexts.
- Operators receive supported operand types.
- Control-flow paths satisfy required return rules.

**Exam/interview trap:** static type checking does not necessarily mean every error is known before execution. Gradual and dynamic languages can defer some type checks; even statically typed languages may still fail at runtime due to null references, bounds errors, or failed casts.

### Questions with answers

1. Why can a local variable hide a global variable of the same name? Because lexical scope lookup chooses the nearest enclosing declaration.
2. Why can a pointer to a local C variable not safely be returned? Its automatic storage duration ends when the function returns.
3. How many n slots exist during fact(3) before the base case returns? Four active frames: values 3, 2, 1, and 0.
