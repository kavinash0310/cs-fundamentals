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
