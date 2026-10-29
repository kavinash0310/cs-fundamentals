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
