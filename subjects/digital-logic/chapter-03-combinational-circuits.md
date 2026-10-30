# Chapter 3 — Combinational Circuit Design

## 1. Design workflow
A combinational output depends only on present inputs, with no stored state. Define the problem, name inputs/outputs, build the truth table, derive Boolean functions, minimize, draw gates, and verify every row.

## 2. Adders
Half adder: S=A XOR B; C=AB. Full adder adds A,B,Cin:
~~~text
S = A XOR B XOR Cin
Cout = AB + ACin + BCin
~~~
A ripple-carry adder chains full adders. It is simple but carry delay grows with bit width. Faster adders compute carries in parallel at greater area/complexity.

Subtraction A-B can be implemented as A + two's-complement(B): invert B and set initial carry to 1.

## 3. Multiplexers, decoders, encoders
A 2-to-1 MUX with select S outputs Y=S'I0+SI1. A 2ⁿ-to-1 MUX uses n select bits and can implement arbitrary functions by choosing data inputs.

A decoder activates one of 2ⁿ outputs corresponding to an n-bit input. An encoder maps an active input to a binary code; a priority encoder resolves multiple active inputs according to priority.

A demultiplexer routes one input to one of multiple outputs. These components can be combined to implement arithmetic and control logic.

## 4. Comparators and arithmetic blocks
Equality of n-bit words is AND of bitwise XNORs. A magnitude comparator examines the most significant differing bit. ALU control selects arithmetic/logic operation; result flags may include zero, carry, sign, and overflow, with definitions dependent on ISA.

## 5. Worked design: majority
Output is 1 when at least two of A,B,C are 1:
~~~text
F = AB + AC + BC
~~~
Truth table check: 000→0, 001→0, 011→1, 111→1. The function is symmetric, so permuting inputs does not change it.

## 6. Questions and practice
**Q1.** What does a MUX select line do? **A:** Chooses which data input reaches output.
**Q2.** Half-adder carry? **A:** AB.
**Q3.** Why does ripple-carry delay scale with bits? **A:** Carries may propagate through each stage.
**Q4.** Design odd parity for three bits. **A:** A XOR B XOR C.
Practice: derive full-adder truth table; make a 4-to-1 MUX from 2-to-1 MUXes; implement a 3-bit equality comparator; design a 2-bit magnitude comparator.

## Notebook work
Draw a full adder from two half adders and an OR gate. Label carry direction in a 4-bit ripple adder.


## Worked adder and selector traces

Full adder with A=1,B=1,Cin=0: sum=0, carry=1. With 1,1,1: sum=1, carry=1. The carry equation AB+ACin+BCin is the majority function.

A 4-to-1 MUX has selects S1S0: 00 chooses I0, 01 I1, 10 I2, 11 I3. To implement a 2-variable function, connect each data input to its truth-table value and use variables as selects.

A 3-bit equality comparator computes (A2 XNOR B2)(A1 XNOR B1)(A0 XNOR B0). The product is 1 only when every bit matches.

Ripple-adder delay example: carry from bit 0 may need to traverse all 31 preceding stages in a 32-bit adder. Carry-lookahead computes group generate/propagate to shorten this dependency at extra hardware cost.

**Practice:** Derive carry for half adder; trace 0111+0001; make odd parity for inputs 101 (output 0); implement F(A,B)=A'B+AB' with a MUX.