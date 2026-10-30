# Chapter 4 — Intermediate Code, Optimization, and Dataflow

## 1. Intermediate representation
IR may be syntax trees, control-flow graphs (CFGs), or three-address instructions. A basic block is a straight-line sequence with one entry and exit. CFG nodes are blocks; edges represent possible control flow.

## 2. Local optimizations
- Constant folding: 3*4 becomes 12.
- Constant propagation: if x=5, replace later uses of x when safe.
- Common-subexpression elimination: reuse a+b if operands have not changed.
- Dead-code elimination: remove computations whose results are unused and have no side effects.
- Strength reduction: replace expensive operation with cheaper equivalent when valid.

Optimization must preserve program behavior, including side effects, exceptions, overflow semantics, and memory aliasing.

## 3. Dataflow analysis
Liveness asks whether a variable's current value may be used later. For block B:
~~~text
OUT[B] = union IN[successors]
IN[B] = USE[B] union (OUT[B] - DEF[B])
~~~
Iterate these equations to a fixed point. A variable live-out must be preserved; a dead value can be overwritten.

Reaching definitions is a forward analysis; available expressions and liveness are classic dataflow problems.

## 4. Questions and practice
**Q1.** When is common-subexpression reuse unsafe? **A:** If operands change or aliasing/side effects change the value.
**Q2.** What is a basic block? **A:** Straight-line code with single entry/exit.
**Q3.** Liveness is forward or backward? **A:** Backward, since future uses flow toward earlier points.
Practice: find leaders and basic blocks; compute one liveness iteration; remove dead assignments; explain why a function call may block an optimization.

## Notebook work
Draw a CFG and calculate USE, DEF, IN, OUT for each block.


## Dataflow worked example

~~~text
B1: x = 2
    y = x + 3
    if c goto B2 else B3
B2: z = y
B3: print(y)
~~~
Constant propagation can replace y with 5 if x is not redefined and overflow/side-effect semantics permit. Liveness: at B3, y is live-in because print uses it. At B1, y is live-out along both paths. If a variable is defined but never used before redefinition, that assignment is dead.

Common-subexpression elimination: in t1=a*b; t2=a*b, reuse t1 only if neither a nor b changes and expression has no side effects. Memory loads are harder because another store or alias may change memory.

Dataflow equations are monotone over finite sets, so iterative transfer to a fixed point terminates. Worklist algorithms process only blocks affected by changed facts.

**Practice:** Mark leaders and basic blocks for a conditional; calculate USE/DEF; propagate a constant through a branch; explain why function calls complicate memory-value reuse.

## Worked notebook: basic blocks, liveness, and safe optimization

Translate a small fragment into three-address code:

~~~text
x = a * 2
y = x + b
if y < limit goto L
z = y + 0
~~~

One possible intermediate representation is:

~~~text
t1 := a * 2
t2 := t1 + b
if t2 < limit goto L
t3 := t2 + 0
z  := t3
~~~

A **basic block** is a maximal straight-line sequence with one entry and no branch except at the end. Leaders are: the first instruction, every jump target, and the instruction after a jump. Build a control-flow graph by making blocks vertices and adding an edge for each possible transfer. The graph, not source indentation, determines which definitions can reach a point.

### Data-flow example: liveness

A variable is live before an instruction if its current value may be used later before being overwritten. For a block:
OUT[B] = union(IN[S]) over successors S,
IN[B] = USE[B] union (OUT[B] - DEF[B]).
For the statement z := t3, DEF={z} and USE={t3}. If z is never used along any reachable path, that assignment is dead. By contrast, t2 < limit is observable through control flow: removing it changes which path executes, even if it does not print a value.

### Safe local simplifications

- Constant folding: replace 3 * 4 with 12 at compile time.
- Algebraic simplification: x + 0 -> x when the language's arithmetic semantics permit it.
- Copy propagation: after p := q, replace a later use of p by q only while q has not been redefined.
- Dead-code elimination: remove a computation whose value is unused and whose evaluation has no observable side effect.
- Common-subexpression elimination: reuse a*b only when neither operand has changed and exceptions/side effects do not make re-evaluation observable.

The phrase “when semantics permit it” matters. In floating-point arithmetic, overflow, NaN, signed zero, and rounding can make an algebraic identity behave differently from exact integer arithmetic. A function call that looks unused may perform I/O or mutate global state; it is not dead.

### SSA and φ-functions

Static single assignment form gives each variable version exactly one definition: x1 := ..., x2 := .... At a control-flow merge, a φ-function selects the version from the predecessor actually taken:

~~~text
if condition:
    x1 = 10
else:
    x2 = 20
join:
    x3 = φ(x1, x2)
~~~

The φ is a compiler representation of a control-flow choice, not a normal arithmetic operation. SSA makes def-use relationships explicit and helps optimization, but generated machine code still needs ordinary moves or registers.

### Short solved questions

1. Is v := a+b removable if v is unused but integer overflow traps? Not automatically; evaluation may raise an observable trap.
2. What changes if q is assigned between p:=q and a use of p? Copy propagation may no longer substitute the new q value.
3. Why can a compiler not freely reorder two writes to the same file? Their observable ordering is part of program behavior.
