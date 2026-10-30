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


## 6. Control-signal reasoning

A single-cycle datapath can be understood by tracing source operands and destinations:

| Instruction | ALU work | Memory | Register write | PC choice |
|---|---|---|---|---|
| Register add | combine two register values | none | ALU result | sequential |
| Load | base + offset | read | memory value | sequential |
| Store | base + offset | write | none | sequential |
| Conditional branch | compare/subtract | none | none | target if condition |

Each mux selects among alternative values. For example, load chooses memory data for register writeback; an arithmetic instruction chooses ALU result. Control must prevent unintended writes on a store or branch.

Signed overflow example using 8 bits: 100 + 60 = 160 mathematically, outside signed range [-128,127]. Binary addition produces 10100000, which reads as -96; same-sign positive inputs yielded a negative result, so overflow=1. By contrast, carry-out can occur without signed overflow.

A microinstruction can specify several compatible control actions in one cycle. The control store maps an instruction/state to a sequence; sequencing logic chooses the next microinstruction.

**Questions**
1. For a load, which value selects register writeback? Memory output.
2. Does a branch normally write the register file? No.
3. Can carry and signed overflow differ? Yes; they answer different arithmetic questions.
4. Why can two's complement subtract with an adder? A-B=A+~B+1.

## 7. Trace practice

For a store at address base 200 plus offset 16, effective address is 216; the data comes from a source register and is written to memory. For branch-if-zero, the ALU/test logic checks the condition; the PC mux chooses target or fall-through. Draw both paths and mark unused components too.