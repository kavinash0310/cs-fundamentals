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

## Extended lecture: follow an instruction through the datapath

For a single-cycle MIPS-like datapath, every instruction completes in one long clock period. The period must be long enough for the slowest instruction, often a memory load. This is simple to reason about but wastes time on simpler operations.

### Load instruction trace

For lw r2, 12(r1):
1. Fetch instruction from instruction memory using PC.
2. Decode opcode and read r1 from the register file.
3. Sign-extend immediate 12 and add it to r1 in the ALU.
4. Read data memory at the effective address.
5. Select memory output, rather than ALU result, for register write-back to r2.
6. Select PC+4 as the next address unless control flow says otherwise.

~~~text
PC -> instruction memory -> decode/register read
                         -> ALU(base + offset) -> data memory
                         -> write-back mux -> register file
~~~

For sw, the ALU still computes base+offset, but register write is disabled; the second register's value goes to memory. For beq, subtract/compare register operands, and choose between PC+4 and the branch target based on the zero result. A mux is essential wherever one hardware resource receives values from different instruction paths.

### Control signals are a decoder

The opcode and function bits drive control choices such as RegWrite, MemRead, MemWrite, MemToReg, ALUSrc, Branch, and ALUOp. For a load: RegWrite=1, MemRead=1, MemWrite=0, MemToReg=memory, ALUSrc=immediate. For a store: RegWrite=0, MemRead=0, MemWrite=1. A wrong control signal can corrupt architectural state even when the ALU arithmetic is correct.

The ALU computes selected operations (add, subtract, AND, OR, compare). ALU control is typically derived from a high-level ALUOp plus instruction function bits. Sign extension preserves a negative two's-complement immediate; zero extension is used for unsigned fields and must not be interchanged casually.

### Timing reasoning

Single-cycle clock period is at least the longest instruction path, including component delays and muxes. If load latency is 800 ps and an ALU instruction path is 500 ps, all instructions still take 800 ps. Multi-cycle designs reuse hardware over several shorter cycles; pipelined designs overlap instructions, with different control and hazard problems.

### Questions

- Why does a store not set RegWrite? Its architectural destination is memory, not a register.
- Which value is added for an address calculation? Base register plus sign-extended displacement.
- Why is the branch decision not just the opcode? The opcode selects branch behavior, while the ALU comparison supplies the condition.
