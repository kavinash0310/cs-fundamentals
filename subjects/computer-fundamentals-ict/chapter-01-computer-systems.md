# Chapter 1 — Computer Systems, Data Flow, and Performance

**Course:** Computer Fundamentals and ICT  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** None

## What you will learn

By the end of this chapter, you should be able to describe a computer as a complete system, distinguish data from information, trace how an input becomes an output, identify core hardware and software, explain the stored-program idea, and solve basic CPU-time questions.

---

## 1.1 A computer system is more than a CPU

A **computer system** is an organised combination of hardware, software, data, procedures, and people that accepts input and produces useful results.

- **Hardware** is the physical equipment: processor, memory, storage, keyboard, display, network interface, and cables.
- **Software** is the set of programs that tells hardware what operations to perform.
- **Data** are the symbols, measurements, and facts a program operates on.
- **People** set objectives, supply or interpret data, create programs, and use results.
- **Procedures** describe how people and systems should carry out recurring work.

A useful exam model is IPO plus storage:

~~~mermaid
flowchart LR
    I[Input data and instructions] --> P[Processing]
    P --> O[Output information]
    P <--> M[Working memory]
    M <--> S[Persistent storage]
    H[People and procedures] -. define and use .-> I
    H -. interpret .-> O
~~~

**Worked example — a school attendance system**

1. A teacher marks who is present: input.
2. A program checks the entries and counts attendance: processing.
3. A register shows attendance totals: output.
4. The day's record is saved: storage.
5. School rules define what counts as present: procedure.
6. The teacher checks exceptions and acts on the report: human judgement.

A computer may calculate the total accurately while the result is still wrong because an absent student was marked present. Correct processing cannot repair incorrect input automatically.

### Data, information, and knowledge

- **Data:** raw facts such as 70, 82, 91.
- **Information:** organised data that answers a question, such as “the average is 81”.
- **Knowledge:** understanding used to interpret information and make a decision, such as deciding that a student needs support in one subject.

The DIKW pyramid is a teaching model, not a rule that every data item must pass through four separate technical stages.

~~~mermaid
flowchart BT
    D[Data: raw observations] --> I[Information: organised meaning]
    I --> K[Knowledge: interpreted understanding]
    K --> W[Wisdom: judgement and action]
~~~

---

## 1.2 The stored-program idea

In a **stored-program computer**, program instructions and the data they use are represented in memory. The CPU repeatedly obtains instructions, interprets them, and performs operations.

The basic cycle is:

~~~mermaid
flowchart LR
    F[Fetch instruction] --> D[Decode operation]
    D --> E[Execute]
    E --> W[Write result if required]
    W --> F
~~~

This simple model is the basis for understanding a program's execution. Real processors use pipelines, caches, prediction, and multiple execution units, but the logical work still includes finding instructions, understanding them, and producing results.

### Von Neumann and Harvard models

- In the **Von Neumann model**, instructions and data share a memory system and usually share paths to the CPU. Competition for those paths can limit transfer speed; this is called the Von Neumann bottleneck.
- In the **Harvard model**, instruction and data memories or paths are separate, allowing some designs to fetch instructions and data simultaneously.
- Many modern processors use a **modified Harvard design** internally, for example separate instruction and data caches backed by shared main memory.

Do not confuse a model of memory organisation with a specific brand or generation of computer.

---

## 1.3 Main hardware blocks

~~~mermaid
flowchart LR
    IN[Input devices] --> MM[Main memory]
    MM <--> CPU[CPU]
    CPU --> OUT[Output devices]
    MM <--> SSD[Secondary storage]
    NET[Network interface] <--> MM
    subgraph CPU
      CU[Control unit]
      ALU[Arithmetic logic unit]
      R[Registers]
    end
~~~

- **CPU (Central Processing Unit):** executes instructions and coordinates operations.
- **ALU (Arithmetic Logic Unit):** performs arithmetic and logical operations.
- **Control unit:** fetches and decodes instructions and sends control signals.
- **Registers:** tiny, fast storage locations inside the CPU.
- **Main memory:** holds instructions and data currently in use.
- **Secondary storage:** retains files and programs when power is off.
- **Input/output devices:** exchange information between the computer and its environment.
- **Network interface:** sends and receives data over a network.

A block diagram explains function, not physical size or the exact wiring of one model.

### Buses

A **bus** is a communication path shared by components.

- **Data bus:** carries data values.
- **Address bus:** identifies a memory location or device register.
- **Control bus:** carries signals such as read, write, interrupt, and timing controls.

A bus may be serial or parallel and may have additional signalling lines. Do not assume every modern connection is a simple set of three wires; the three-bus names are a useful functional model.

---

## 1.4 Computer categories and generations

### Categories by common role

| Category | Typical role | Example |
|---|---|---|
| Supercomputer | Extremely large scientific computation | Climate simulation |
| Mainframe | High-volume transaction processing and many concurrent users | Banking core systems |
| Server | Supplies data or services to clients | Web or database server |
| Workstation | High-performance professional use | Engineering design |
| Personal computer | General-purpose individual use | Desktop or laptop |
| Mobile device | Portable personal computing | Phone or tablet |
| Embedded system | Computer built into a larger device | Router or appliance controller |

These labels are about common use rather than absolute performance rankings. A server can run on a small physical machine, and a workstation can outperform an old mainframe on a particular task.

### Generations — conventional textbook grouping

| Generation | Main electronic technology | Typical effect |
|---|---|---|
| First | Vacuum tubes | Large, hot, power hungry |
| Second | Transistors | Smaller and more reliable |
| Third | Integrated circuits | Many components placed on a chip |
| Fourth | Microprocessors and VLSI | Personal computers became widespread |
| Fifth | Often described using AI or advanced processing | Textbook label; not a precise universal hardware boundary |

Recall the sequence V-T-I-M: vacuum tube, transistor, integrated circuit, microprocessor. Dates and the fifth-generation description vary across textbooks, so follow the exam's prescribed text when a question expects a specific convention.

---

## 1.5 System software and application software

- **System software** manages the computer or provides a platform for other software. Examples: operating systems, device drivers, and language runtimes.
- **Application software** helps users perform tasks. Examples: spreadsheet, browser, media editor, and school result application.
- **Firmware** is low-level software stored in non-volatile memory and used to control a device.
- **Utility software** performs maintenance or protection tasks such as backup, compression, or malware scanning.

The same physical machine can run many different applications because its operating system and hardware provide reusable services.

---

## 1.6 Representing information

Hardware operates on electrical states that are treated as binary values, usually 0 and 1. A **bit** is one binary digit. A byte is commonly eight bits.

The bit pattern has no inherent meaning without a representation rule. The same 8-bit pattern could mean:

- an unsigned integer;
- a signed integer;
- a character code;
- part of an image;
- an instruction, depending on context.

This is why data types, file formats, and communication protocols matter: they define how bit patterns should be interpreted. Number systems and data representation are treated fully in Chapter 2.

---

## 1.7 Measuring processor performance

A common first-order CPU execution-time model is:

~~~text
CPU time = instruction count × average CPI ÷ clock rate
~~~

- **Instruction count (IC):** number of machine instructions executed for the program.
- **CPI:** average clock cycles per instruction for the chosen workload and processor.
- **Clock rate:** cycles per second, measured in hertz.

### Worked numerical example

A program executes 1.2 billion instructions. Its average CPI is 1.5 and the processor clock is 2 billion cycles per second.

~~~text
CPU time = (1.2 × 10^9 instructions × 1.5 cycles/instruction)
           ÷ (2 × 10^9 cycles/second)
         = 1.8 × 10^9 ÷ 2 × 10^9 seconds
         = 0.9 seconds
~~~

The units cancel: instructions × cycles/instruction gives cycles; cycles ÷ cycles/second gives seconds.

### Comparing two implementations

Suppose implementation A needs 900 million instructions at CPI 2 and 3 GHz:

~~~text
Time A = (0.9 × 10^9 × 2) ÷ (3 × 10^9) = 0.6 seconds
~~~

Implementation B has fewer instructions but a larger CPI. Compare total CPU time, not instruction count or clock rate in isolation.

**Important limitation:** This model assumes the stated average CPI and ignores I/O wait, cache effects, operating-system activity, and other system delays. Real benchmarking should use the same workload and conditions.

---

## 1.8 Short worked reasoning: input errors and system quality

A result system calculates 100% attendance for a class, but the paper register shows one student absent.

Trace the system:

1. Was the data entered correctly?
2. Did the program count the correct field?
3. Was the report generated from the latest data?
4. Is the output being interpreted correctly?
5. Does the school procedure define attendance consistently?

This is a systems investigation. Do not blame the CPU when the fault may be data entry, software logic, a stale file, or a procedure.

---

## 1.9 Questions and answers

1. **What is a computer system?**  
   Hardware, software, data, people, and procedures organised to process information.

2. **How are data and information different?**  
   Data are raw facts; information is data organised to answer a question.

3. **What does IPO mean?**  
   Input, processing, and output. Storage supports the cycle.

4. **What is a stored-program computer?**  
   A system that stores instructions in memory so the processor can fetch and execute them.

5. **What does the control unit do?**  
   It coordinates operations by fetching and decoding instructions and issuing control signals.

6. **What does the ALU do?**  
   It performs arithmetic and logical operations.

7. **Name the three functional bus categories.**  
   Data, address, and control.

8. **Why is a high clock rate not enough to compare processors?**  
   Instruction count, CPI, architecture, memory, workload, and I/O also affect performance.

9. **What is the Von Neumann bottleneck?**  
   A limit caused when instructions and data compete for shared memory paths.

10. **Can an accurate computer produce a wrong result?**  
    Yes. Incorrect input, specifications, logic, or interpretation can make the overall result wrong.

### GATE-style practice with solutions

**1. CPU-time calculation.** A program executes 600 million instructions at CPI 2.5 on a 3 GHz CPU. Find CPU time.

~~~text
Time = (600 × 10^6 × 2.5) / (3 × 10^9)
     = 1500 × 10^6 / 3000 × 10^6
     = 0.5 seconds
~~~

**2. Compare execution times.** Program P uses 1 billion instructions at CPI 1.2 on a 2 GHz processor. Program Q uses 700 million instructions at CPI 2 on the same processor.

~~~text
P: (1 × 10^9 × 1.2) / (2 × 10^9) = 0.6 seconds
Q: (0.7 × 10^9 × 2) / (2 × 10^9) = 0.7 seconds
P is faster despite using more instructions.
~~~

**3. Concept check.** A computer with a very fast CPU takes a long time to load a file from a remote server. Is CPU time the only relevant measure?  
**Answer:** No. Network delay, server response, storage, and software processing all contribute to elapsed time.

### Multiple-choice check

1. Which is not hardware? A. CPU B. RAM C. Operating system D. Keyboard  
2. A byte commonly contains: A. 4 bits B. 8 bits C. 16 bits D. 32 bits  
3. Which bus identifies a memory location? A. Data B. Address C. Control D. Power  
4. Which category commonly provides services to clients? A. Server B. Scanner C. ALU D. Register  
5. Which expression is the standard CPU-time model?  
   A. IC × CPI ÷ clock rate  
   B. IC + CPI + clock rate  
   C. clock rate ÷ IC × CPI  
   D. IC × clock rate ÷ CPI

**Answers:** 1-C, 2-B, 3-B, 4-A, 5-A. An operating system is software; the other answers identify hardware or the standard performance formula.

### Draw-from-memory task

Draw the input → main memory ↔ CPU → output flow, add secondary storage, then label the CPU's ALU, control unit, and registers. Explain one student's attendance record through each block.
