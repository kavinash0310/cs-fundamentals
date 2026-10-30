# Chapter 1 — Performance, Instructions, and Addressing

## Learning goals
Relate program execution to the ISA, calculate CPU time, trace basic instruction formats, and distinguish addressing modes.

## 1. ISA and machine state
An instruction-set architecture defines programmer-visible operations: registers, instruction encodings, data types, memory model, and control flow. Microarchitecture is the internal implementation. Multiple processors can implement the same ISA differently.

Architectural state includes registers, memory, and program counter (PC). The fetch-decode-execute cycle reads the instruction at PC, decodes operation and operands, executes, writes result, then updates PC or branches.

## 2. Instructions and addressing modes
An instruction may contain opcode, register fields, immediate, and address/displacement. Common modes:
- Immediate: operand is in instruction.
- Register: operand is in named register.
- Direct/absolute: instruction gives memory address.
- Register indirect: register contains address.
- Base+offset: effective address = base register + displacement.
- PC-relative: target = PC + signed offset.

Example: load R1, 8(R2) accesses memory at R2+8. Immediate 8 means the value 8 itself; it is not a memory address.

## 3. CPU-time equation
CPU time = instruction count × average CPI × clock-cycle time = IC×CPI/frequency. CPI depends on instruction mix and microarchitecture; clock rate alone does not compare performance.

Example: 2 billion instructions at CPI 1.5 and 3 GHz take (2e9×1.5)/(3e9)=1 second. If a change halves IC but doubles CPI, compare the product.

Amdahl's law: speedup = 1/[(1-p)+p/s], where p is improved fraction and s is its speedup. Improving a small fraction has limited overall effect.

## 4. Questions and practice
**Q1.** What does an ISA specify? **A:** Programmer-visible instructions/state/behavior.
**Q2.** Address in 12(R4)? **A:** R4+12.
**Q3.** CPU time for 600M instructions, CPI 2, 2GHz? **A:** 0.6 s.
**Q4.** Why isn't clock rate enough? **A:** Work per instruction and instruction count also matter.
Practice: classify operands by mode; compare two machines with different IC/CPI/clock; apply Amdahl to an optimization.

## Notebook work
Draw fetch-decode-execute and mark where PC changes on a branch.


## 5. Worked instruction and performance analysis

Assume an instruction uses base-plus-offset addressing. The instruction is LOAD R3, 12(R5), R5=1000. The effective byte address is 1012; the loaded value is whatever memory stores there. Do not confuse the immediate offset 12 with the loaded data. If the architecture addresses words, an offset may be scaled; the ISA defines that detail.

Machine A executes 1.2 billion instructions at average CPI 1.5 and 2.4 GHz:
~~~text
time = 1.2e9 * 1.5 / 2.4e9 = 0.75 seconds
~~~
Machine B executes the same program in 0.60 seconds, so B is 1.25 times as fast for this workload. Benchmark conclusions depend on workload and compiler-generated instruction count.

**Addressing-mode check:** In immediate mode the operand value is part of the instruction. In register-indirect mode a register holds the effective address. In base+offset mode the effective address is computed from a base plus displacement. A branch displacement is commonly added to PC, but whether PC refers to current or next instruction is ISA-specific.

**Practice with answers**
1. 900 million instructions, CPI 2, 1.5 GHz: time = 1.2 s.
2. If an optimization speeds 40% of execution by 2x, total speedup = 1/(0.6+0.4/2)=1.25.
3. Which matters when comparing instruction sets? Instruction count, CPI, and cycle time, not any one alone.
4. A program uses fewer instructions but slower average CPI. Compare products IC×CPI before deciding.

## 6. Draw-from-memory check

Draw PC, instruction memory, register file, ALU, and data memory. Trace a load and a branch in separate colors. Write the CPU-time equation beside the diagram.

## Extended lecture: compare processors without guessing

CPU time is approximately: instruction count × average CPI × clock-cycle time. Since clock-cycle time is 1/clock rate, CPU time = IC×CPI/clock rate. Every comparison must name the workload and units. A processor with a higher clock rate can still be slower if its program uses more instructions or takes more cycles per instruction.

**Worked comparison.** Program A executes 2×10^9 instructions at CPI 1.5 and 3 GHz: time=(2×10^9×1.5)/(3×10^9)=1 s. Program B executes 1.5×10^9 instructions at CPI 2 and 2 GHz: time=1.5 s. B has fewer instructions but is slower. MIPS is not a reliable cross-ISA comparison because instruction counts and work per instruction differ.

Amdahl's law limits speedup when only part of a task improves. If fraction f is accelerated by factor s, total speedup = 1/((1-f)+f/s). If 80% of execution becomes 4× faster, speedup=1/(0.2+0.8/4)=2.5×, not 4×. The untouched 20% sets a hard ceiling of 5× even with infinite improvement.

### Addressing-mode calculation

For a 32-bit instruction with a 6-bit opcode and two 5-bit register fields, the remaining immediate field is 32-6-5-5=16 bits. A signed 16-bit immediate represents -32768 through 32767. In a base-plus-offset load, effective address = register value + sign-extended offset. If base=0x1000 and offset=-12, address=0x0FF4. The offset is measured in bytes unless the ISA specifies scaling.

### Essential distinction

ISA is the programmer-visible contract: instructions, registers, memory behavior, exceptions, and encodings. Microarchitecture is one implementation of that contract: pipeline depth, caches, execution units, and prediction. Multiple processors can implement the same ISA with very different performance and energy.

### Self-test with answers

1. Which improves time: reducing CPI by 10% or clock period by 10%, with other values fixed? Either reduces CPU time by 10%.
2. If a fraction f=0.5 is made 2× faster, total speedup=1/(0.5+0.25)=1.333×.
3. Why can MIPS mislead? It ignores how much useful work each ISA instruction represents.
