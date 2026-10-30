# Chapter 3 — Pipelining and Hazards

## 1. Pipeline idea
Pipelining overlaps instruction stages, like an assembly line. A classic five-stage model is IF (fetch), ID (decode/register read), EX (execute/address), MEM (data memory), WB (write back). Throughput improves after the pipeline fills; a single instruction's latency may not shrink.

Ideal speedup approaches number of stages only when stages are balanced and hazards absent. Real CPI is affected by stalls, flushes, and unequal stage delays.

## 2. Structural, data, and control hazards
- Structural: two operations need the same hardware resource simultaneously.
- Data: an instruction needs a value not yet produced.
- Control: next PC depends on branch/jump outcome.

For RAW dependency, forwarding may send a result directly to a later stage. A load-use dependency can still require a stall if data arrives too late. WAR/WAW hazards arise in some deeper or out-of-order pipelines; classic in-order five-stage pipeline primarily faces RAW.

## 3. Branches and performance
A taken branch can flush younger instructions. Prediction reduces control penalty; misprediction loses work. Branch delay depends on when target and condition are known.

If base CPI=1 and 20% instructions incur 2 stall cycles, CPI=1+0.2×2=1.4. Execution time is IC×CPI×cycle time.

## 4. Questions and practice
**Q1.** Does pipelining reduce each instruction's latency? **A:** Not necessarily; it primarily improves throughput.
**Q2.** Name three hazards. **A:** Structural, data, control.
**Q3.** What does forwarding solve? **A:** Many RAW dependencies without waiting for register writeback.
**Q4.** CPI with 10% branches, half mispredicted at 3-cycle penalty, base 1? **A:** 1+0.1×0.5×3=1.15.
Practice: mark dependencies in an instruction sequence; show where a load-use stall occurs; calculate speedup with a branch penalty.

## Notebook work
Draw five overlapping instruction timelines and shade a data stall and a branch flush.


## 5. Pipeline timing and hazards in detail

For k equal stages of latency τ and n instructions, ideal pipelined time is approximately (k+n-1)τ; an unpipelined machine takes nkτ. For large n, ideal speedup approaches k. Unequal stage delays, latch overhead, stalls, and flushes reduce this.

Example five-stage pipeline:
~~~text
cycle:  1  2  3  4  5  6  7
I1:    IF ID EX MEM WB
I2:       IF ID EX MEM WB
I3:          IF ID EX MEM WB
~~~
One instruction completes per cycle after fill, but each still traverses five stages.

RAW example: I1 computes R1; immediately following I2 uses R1. Forwarding sends the produced value from a later pipeline stage to I2's EX input. If I1 is a load, memory data may arrive too late for the next cycle; insert a bubble, then forward.

~~~text
load R1,[R2]
add  R3,R1,R4
cycle 1: IF
cycle 2:    ID
cycle 3:       EX(load)   ID(add)
cycle 4:          MEM     ID(add held / bubble)
cycle 5:          WB      EX(add using forwarded value)
~~~

A structural hazard can be removed by separate instruction/data memories or by stalling. A branch hazard can be handled by stall, predict-not-taken, or dynamic prediction; a wrong prediction flushes speculative instructions.

**CPI model:** CPI = base CPI + average stall cycles/instruction. If 25% of instructions are loads and 20% of those cause one stall, load stalls add 0.25×0.20×1=0.05 CPI.

**Practice**
1. Five-stage, 100 instructions, no stalls: ideal cycles = 5+100-1=104.
2. If 12 branches per 100 instructions each incur 2 lost cycles, added CPI=0.24.
3. A dependency is not automatically a stall if forwarding supplies the value in time.
4. Distinguish latency (time for one instruction) from throughput (completion rate).

## Extended lecture: schedule a five-stage pipeline

A classic pipeline divides work into IF (fetch), ID (decode/read registers), EX (execute/address), MEM (data-memory access), and WB (write result). Separate pipeline registers hold values/control between stages. With balanced stages, ideal throughput approaches one instruction per cycle after filling, but latency for one instruction still spans about five stages.

For n instructions and k stages with no stalls, cycles = k+n-1. For 8 instructions in 5 stages: 12 cycles, compared with 40 cycles if each instruction took five cycles without overlap. Ideal speedup tends toward k only for a long stream and equal stage delays; real hazards and unequal stages reduce it.

### Data hazards

~~~text
add r3, r1, r2   ; produces r3 in EX
sub r4, r3, r5   ; needs r3 in EX
~~~

The second instruction reads a value produced by the first. Forwarding routes the result directly from a later pipeline register to the ALU input, avoiding a register-file wait. A load-use hazard is harder:

~~~text
lw  r3, 0(r1)
add r4, r3, r5
~~~

The load's data is available only after MEM, too late for the immediately following instruction's EX stage. A simple pipeline inserts one bubble, then forwards the loaded value. Compilers can sometimes schedule an independent instruction into the gap.

### Structural and control hazards

A structural hazard occurs when simultaneous stages need the same single-ported resource; duplicated resources or a stall can resolve it. A branch creates a control hazard because the correct next PC may be unknown until comparison. A processor may stall, predict, or resolve the branch earlier. A misprediction flushes younger wrong-path instructions; their speculative results must not commit architecturally.

### Timing diagram (forwarding used)

~~~text
cycle     1   2   3   4   5   6
add       IF  ID  EX  MEM WB
sub           IF  ID  EX  MEM WB
                    ^ forwarded operand
~~~

### Exam traps

- Pipelining improves throughput, not necessarily the latency of one instruction.
- A bubble occupies a stage slot but is not a useful instruction.
- Forwarding fixes many RAW hazards but cannot make unavailable load data appear early.
- WAR and WAW hazards are not present in the simple in-order five-stage pipeline, but can arise with out-of-order execution.

### Check yourself

For n=20 instructions, k=5, ideal cycles=24. Why not 100? Stages overlap. If there are 3 stall cycles, total=27 cycles before any additional branch penalties.
