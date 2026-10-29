# Chapter 2 — Datapath, ALU, and Control Unit

## 1. Datapath components
The datapath moves and transforms values. Common components are register file, ALU, PC, instruction memory, data memory, sign extender, shifter, and multiplexers. Control signals select sources, operations, and write enables.

~~~text
registers -> operand mux -> ALU -> result mux -> registers
                         |                 |
                         +-> data memory <-+
PC -> instruction fetch -> decode/control
~~~

A load reads memory and writes a register; a store reads a register and writes memory. An arithmetic instruction selects register operands and ALU operation. A branch compares operands and may choose a new PC.

## 2. ALU operations
An ALU may compute add/subtract, bitwise AND/OR/XOR, shifts, and comparisons. For two's-complement addition, sign extension preserves value when widening. Overflow flags are distinct from carry: signed overflow occurs if same-sign addends produce opposite-sign result.

For subtraction A-B, compute A+~B+1. For a 32-bit ALU, control bits select operation; output flags can indicate zero, carry, negative, and overflow.

## 3. Single-cycle versus multi-cycle
A single-cycle design completes each instruction in one long clock period; slowest instruction determines cycle time. Multi-cycle design reuses units over several shorter cycles and uses an FSM controller. Single-cycle control is conceptually simple but hardware resources may be duplicated.

## 4. Control unit
Hardwired control uses combinational logic from opcode/state to control signals; it is fast but changes can be complex. Microprogrammed control stores sequences of microinstructions in control memory; flexible and easier to modify, typically with added control-store access.

## 5. Worked load trace
For LOAD R1, 8(R2): decode opcode and registers; ALU adds base R2 and immediate 8; data memory reads effective address; result mux selects memory data; register file writes R1. PC advances unless control flow changes.

## 6. Questions and practice
**Q1.** Which unit computes an effective base+offset address? **A:** ALU.
**Q2.** Why use a mux? **A:** Select among alternative datapath values.
**Q3.** Hardwired vs microprogrammed control? **A:** Logic network vs stored control sequence.
Practice: trace store, branch, and add instructions; sketch required control signals; distinguish carry and signed overflow.

## Notebook work
Draw a load datapath and highlight the active route in a different color.
