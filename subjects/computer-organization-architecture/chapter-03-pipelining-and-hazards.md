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
