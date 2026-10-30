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