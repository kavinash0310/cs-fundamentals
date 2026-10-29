# Chapter 3 — CPU, Instructions, and Processor Performance

**Course:** Computer Fundamentals and ICT  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–2

## What you will learn

This chapter explains how a processor represents and executes instructions. You will distinguish an ISA from an implementation, trace a simple instruction sequence, use common addressing modes, compare control-unit designs, and solve CPU performance and pipeline questions.

---

## 3.1 ISA, implementation, and microarchitecture

An **Instruction Set Architecture (ISA)** is the programmer-visible contract between software and a processor. It specifies items such as:

- instructions and their operation codes;
- registers available to programs;
- data types and operand sizes;
- addressing modes;
- memory and input/output behavior visible to software;
- exceptions and privilege rules.

A **microarchitecture** is one internal design that implements an ISA. Different processors can implement the same ISA using different pipelines, caches, execution units, and control logic.

**Example:** Two processors may both run the same machine-code program because they implement a compatible ISA, even though one has more cores and a different cache design.

An **instruction** usually describes an operation and its operands. A simplified instruction format may contain:

~~~text
[operation code][register field][address or immediate field]
~~~

The exact fields, sizes, and binary encoding depend on the ISA.

---

## 3.2 CPU datapath and key registers

The **datapath** contains the registers, ALU, buses, and other circuits that move and transform values.

Common conceptual registers:

- **PC (Program Counter):** address of the next instruction to fetch.
- **IR (Instruction Register):** instruction currently being decoded or executed.
- **MAR (Memory Address Register):** address used for a memory access in a simple textbook model.
- **MDR/MBR (Memory Data/Buffer Register):** data being read from or written to memory in that model.
- **General-purpose registers:** hold operands, addresses, and intermediate results.
- **Status/flag register:** records conditions such as zero, carry, sign, and overflow.
- **Stack pointer:** identifies the top of a stack used for calls, returns, and temporary values.

Modern processors do not always expose these textbook registers as physical named registers. They remain useful for understanding the work.

### Datapath sketch

~~~mermaid
flowchart LR
    PC[Program counter] --> FETCH[Instruction fetch]
    FETCH --> IR[Instruction register]
    IR --> CU[Control unit]
    R[Register file] <--> ALU[ALU]
    IMM[Immediate value] --> ALU
    ALU --> R
    R <--> MEM[Memory interface]
    CU -. control signals .-> R
    CU -. select operation .-> ALU
    CU -. read or write .-> MEM
~~~

---

## 3.3 Fetch, decode, and execute in more detail

A simplified memory-read cycle:

1. Copy the address from PC to MAR.
2. Issue a memory read.
3. Copy the fetched instruction from MDR to IR.
4. Advance PC to the next instruction, unless the ISA or instruction changes control flow.
5. Decode the operation and identify source/destination operands.
6. Read operands from registers or memory.
7. Execute in an ALU or another functional unit.
8. Write the result and update relevant flags.
9. Check for exceptions or interrupts according to the architecture.

The sequence is conceptual; processors overlap, reorder, or combine operations internally while preserving required program behavior.

### Worked trace: add two memory values

Suppose a toy machine has:

- LOAD R1, [A] meaning read memory location A into R1;
- ADD R1, R2 meaning R1 = R1 + R2;
- STORE [C], R1 meaning write R1 to memory location C.

Initial state: A contains 7 and B contains 5.

~~~text
LOAD R1, [A]     R1 = 7
LOAD R2, [B]     R2 = 5
ADD R1, R2       R1 = 12
STORE [C], R1    C = 12
~~~

The instruction set is invented for illustration; real assembly syntax differs.

---

## 3.4 Addressing modes

An **addressing mode** describes how an instruction finds an operand.

| Mode | Meaning | Example-style interpretation |
|---|---|---|
| Immediate | Operand value is inside the instruction | ADD R1, #5 |
| Register | Operand is in a named register | ADD R1, R2 |
| Direct/absolute | Instruction contains a memory address | LOAD R1, [1000] |
| Register indirect | Register contains the memory address | LOAD R1, [R2] |
| Indexed/base plus displacement | Effective address = base/index register + constant | LOAD R1, [R2 + 12] |
| PC-relative | Target = PC + signed displacement | Branch if zero, offset +20 |
| Stack/implicit | Operand is implied by stack or instruction | PUSH, POP, or accumulator operation |

### Effective-address worked example

A base register contains address 1000. An instruction uses displacement 24.

~~~text
Effective address = base + displacement
                  = 1000 + 24
                  = 1024
~~~

For a PC-relative branch, if the architecture defines the base as the address of the next instruction, use that address plus the signed displacement. Read the convention stated in the question.

---

## 3.5 Instruction categories

Common instruction groups include:

- **Data transfer:** load, store, move.
- **Arithmetic:** add, subtract, multiply, divide.
- **Logical/bit operations:** AND, OR, XOR, shifts, masks.
- **Control flow:** branch, jump, call, return.
- **Comparison/test:** set flags or produce a Boolean result.
- **System operations:** controlled access to privileged services or processor state.

An instruction set may have many specialised operations. A compiler maps high-level language constructs to sequences of machine instructions.

---

## 3.6 Control unit

The control unit generates signals that coordinate instruction execution.

### Hardwired control

- Control signals are produced by fixed logic circuits.
- Can be fast for a given design.
- Changing the control behavior may require redesigning hardware.
- Common in many RISC-style implementations, though the distinction is not absolute.

### Microprogrammed control

- Control signals are selected by microinstructions stored in a control store.
- A complex machine instruction may be implemented by a sequence of micro-operations.
- Easier to modify or extend in some designs, though control-store access has costs.
- Traditionally associated with some complex instruction sets; modern processors may combine methods.

A **micro-operation** is a small internal step, such as transferring a register value or selecting an ALU operation.

---

## 3.7 RISC and CISC as design traditions

- **RISC (Reduced Instruction Set Computer):** design tradition emphasizing a relatively regular instruction set, many registers, and simple operations that are easy to pipeline.
- **CISC (Complex Instruction Set Computer):** design tradition with richer instructions and addressing forms, sometimes allowing one instruction to perform several steps.

This distinction is historical and conceptual, not a reliable rule that every modern RISC chip is simple or every CISC instruction executes in one complicated circuit. Modern processors often decode instructions into internal micro-operations.

---

## 3.8 Processor performance

The basic execution model is:

~~~text
CPU time = IC × CPI × clock-cycle time
         = IC × CPI ÷ clock rate
~~~

Where:

- IC = dynamic instruction count for the program;
- CPI = average cycles per instruction;
- clock-cycle time = seconds per cycle;
- clock rate = cycles per second.

For a mixture of instruction classes:

~~~text
Average CPI = sum over classes (fraction of instructions in class × CPI of class)
~~~

### Worked example: average CPI

A program has 50% class A instructions at 1 cycle, 30% class B at 2 cycles, and 20% class C at 4 cycles.

~~~text
Average CPI = 0.50×1 + 0.30×2 + 0.20×4
            = 0.5 + 0.6 + 0.8
            = 1.9 cycles/instruction
~~~

If the program executes 100 million instructions on a 1 GHz processor:

~~~text
CPU time = 100×10^6 × 1.9 / 10^9 = 0.19 seconds
~~~

A performance improvement must be judged by the whole execution time. A higher clock rate can be offset by a larger CPI or instruction count.

---

## 3.9 Pipelining

A **pipeline** divides instruction execution into stages and overlaps different instructions. A textbook five-stage pipeline is:

~~~mermaid
flowchart LR
    IF[IF: instruction fetch] --> ID[ID: decode and register read]
    ID --> EX[EX: execute or address calculation]
    EX --> MEM[MEM: memory access]
    MEM --> WB[WB: write back]
~~~

Without pipelining, each instruction may use all stages before the next begins. With pipelining, several instructions are in different stages at the same time.

### Ideal cycle count

For k pipeline stages and n instructions, ideal cycles are:

~~~text
k + n - 1
~~~

assuming every stage takes one cycle and no stalls occur. The non-pipelined count under the same simple assumptions is k × n.

**Worked example:** k=5 and n=10.

~~~text
Pipelined cycles = 5 + 10 − 1 = 14
Non-pipelined cycles = 5 × 10 = 50
Ideal speedup = 50 / 14 ≈ 3.57
~~~

As n becomes very large, ideal speedup approaches k, but actual pipelines lose time to unequal stages, hazards, stalls, branch behavior, and overhead.

### Pipeline hazards

- **Structural hazard:** two operations need the same hardware resource at once.
- **Data hazard:** an instruction needs a value that a preceding instruction has not yet produced.
- **Control hazard:** a branch changes which instruction should execute next.

Common remedies include stalling, forwarding/bypassing, resource duplication, and branch prediction. Detailed pipeline problems are continued in the Computer Organization and Architecture course.

---

## 3.10 Interrupts and exceptions: first distinction

- An **interrupt** is commonly an asynchronous event from an external device or timer that requests processor attention.
- An **exception/trap** is commonly caused synchronously by the instruction being executed, such as an invalid operation or system call.
- Exact terminology is architecture-specific.

The processor saves enough state to handle the event and later resume or redirect execution. Interrupts allow a CPU to respond to devices without continuously checking each one.

---

## 3.11 Questions and answers

1. **What is an ISA?**  
   The programmer-visible instruction and state contract implemented by a processor.

2. **How does ISA differ from microarchitecture?**  
   The ISA specifies visible behavior; microarchitecture describes the internal design that implements it.

3. **What does PC hold?**  
   In the simplified model, the address of the next instruction to fetch.

4. **What is an addressing mode?**  
   A rule for locating an instruction's operand.

5. **What is immediate addressing?**  
   The operand value is encoded in the instruction itself.

6. **What is a register-indirect address?**  
   A register contains the effective memory address.

7. **What does the status register contain?**  
   Condition information such as zero, carry, sign, or overflow flags.

8. **What does a pipeline overlap?**  
   Different stages of multiple instructions.

9. **Name three pipeline hazards.**  
   Structural, data, and control hazards.

10. **Why does pipelining not make one instruction take one fifth of the time in every real processor?**  
    Stage work, dependencies, stalls, branches, and implementation overhead limit ideal overlap.

### GATE-style practice with solutions

**1. Effective address.** Base register = 2400, displacement = −16. Find the effective address.  
**Solution:** 2400 + (−16) = 2384.

**2. Average CPI.** A workload has 60% instructions at CPI 1 and 40% at CPI 3.  
**Solution:** 0.6×1 + 0.4×3 = 1.8 cycles/instruction.

**3. CPU time.** IC = 2×10⁸, CPI = 1.8, clock = 1.5 GHz.  
**Solution:** (2×10⁸×1.8)/(1.5×10⁹) = 0.24 seconds.

**4. Ideal pipeline.** A 4-stage pipeline executes 20 instructions without stalls.  
**Solution:** 4+20−1 = 23 cycles. Non-pipelined = 4×20 = 80 cycles. Ideal speedup ≈ 3.48.

### Multiple-choice check

1. Programmer-visible instruction behavior belongs to: A. ISA B. cache line C. file system D. compiler warning  
2. An instruction's value encoded directly in the instruction uses: A. immediate B. register indirect C. indexed D. stack  
3. A branch creates what kind of pipeline hazard? A. control B. structural only C. data only D. storage  
4. A base of 100 plus displacement 12 gives: A. 88 B. 100 C. 112 D. 120  
5. Ideal five-stage pipeline cycles for eight instructions are: A. 13 B. 20 C. 40 D. 5

**Answers:** 1-A, 2-A, 3-A, 4-C, 5-A.

### Draw-from-memory task

Draw a datapath containing PC, instruction register, control unit, register file, ALU, and memory. Then draw a five-stage pipeline and mark where a load-use data hazard could occur.
