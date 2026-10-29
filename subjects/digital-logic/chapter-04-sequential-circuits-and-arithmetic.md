# Chapter 4 — Sequential Circuits, State, and Computer Arithmetic

## 1. State and clocks
A sequential circuit has memory: output depends on current input and stored state. A clock coordinates state transitions. Synchronous circuits update state at specified clock edges; asynchronous changes can propagate without a global edge.

A latch is level-sensitive; a flip-flop samples on an edge. Timing constraints include setup time before the edge and hold time after it. Violations can cause metastability.

## 2. Flip-flops
SR latch stores a bit but has an invalid input combination in common implementations. D flip-flop stores D at the active edge. JK and T flip-flops support toggle behavior; T toggles when T=1.

A register is a group of flip-flops storing a word. A shift register moves bits per clock and supports serial/parallel conversion.

## 3. Counters and finite-state machines
A counter cycles through states. An n-bit binary counter has 2ⁿ states. Synchronous counters update flip-flops from shared clock; ripple counters propagate toggles and accumulate delay.

FSM design steps: define states, inputs, outputs; draw state diagram; assign binary encodings; derive next-state/output functions; simplify and verify. Moore outputs depend only on state; Mealy outputs depend on state and current input.

Example: a two-state toggle FSM alternates 0→1→0 on every enabled clock. Reset establishes a known start state.

## 4. Fixed and floating-point arithmetic
Fixed-point uses a fixed binary scale; precision and range are limited by chosen bits. Floating point stores sign, exponent, and significand, representing approximately (-1)^s × significand × base^exponent. Rounding means many real values are approximated; overflow and underflow must be considered.

IEEE-style single precision uses 1 sign bit, 8 exponent bits, and 23 fraction bits. Normal values have an implicit leading 1; subnormals allow gradual underflow. NaN and infinities have special exponent patterns. Exact bit patterns depend on exponent bias and fraction.

## 5. Questions and practice
**Q1.** Difference between latch and flip-flop? **A:** Level-sensitive versus edge-triggered storage.
**Q2.** What is setup time? **A:** Minimum input stability before active clock edge.
**Q3.** Moore vs Mealy? **A:** Output depends on state only vs state and input.
**Q4.** Why can floating point fail exact decimal equality? **A:** Many decimal fractions have no finite binary representation.
Practice: draw a 2-bit counter state diagram; trace register contents under shifts; normalize a binary fraction; explain overflow versus rounding.

## Notebook work
Draw a D flip-flop timing diagram, a 3-bit counter sequence, and a floating-point field layout.
