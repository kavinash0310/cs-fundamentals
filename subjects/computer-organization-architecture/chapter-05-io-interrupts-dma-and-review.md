# Chapter 5 — I/O, Interrupts, DMA, and Review

## 1. Programmed I/O and interrupts
With programmed I/O (polling), CPU repeatedly checks a device status register. It is simple but wastes CPU time when the device is slow. Interrupt-driven I/O lets a device signal attention; CPU saves state, runs an interrupt handler, acknowledges the event, then resumes.

Interrupts need priority, masking, and a well-defined save/restore path. An exception is usually caused synchronously by the current instruction; an interrupt is commonly asynchronous, though terminology varies by architecture.

## 2. Direct Memory Access
DMA controller transfers blocks between device and memory with limited CPU involvement. CPU configures address, size, and direction; DMA performs transfer; completion interrupt notifies CPU. DMA reduces per-byte CPU work but competes for memory bus bandwidth and needs cache-coherence consideration.

## 3. Buses and memory interfacing
A bus carries address, data, and control information. Memory interfacing decodes addresses and selects devices. Memory-mapped I/O uses ordinary load/store address space; isolated I/O uses separate instructions/space.

## 4. Performance review
Use IC×CPI×cycle time. Cache affects average access, pipeline hazards affect CPI, and faster clock affects cycle time. Optimizing one factor may worsen another (area, energy, correctness, or compiler scheduling).

### End-of-course questions
**Q1.** Polling vs interrupt? **A:** Repeated CPU checking vs device-triggered service.
**Q2.** Who transfers bulk data in DMA? **A:** DMA controller, with CPU setup/completion.
**Q3.** What improves cache hit rate? **A:** Workload locality and appropriate cache design.
**Q4.** Amdahl's law limit? **A:** Unimproved fraction bounds total speedup.
Practice: trace an interrupt; compare DMA with programmed I/O; calculate CPU time and AMAT for one workload.

## Notebook work
Draw the interrupt path and a DMA transfer timeline. Explain where CPU, device, memory, and controller participate.


## 5. I/O paths and worked comparison

Polling timeline: CPU reads device status repeatedly until ready, then transfers data. Interrupt timeline: CPU starts I/O, runs another task, device raises interrupt, handler acknowledges/completes work. DMA timeline: CPU programs buffer address/length/direction; controller transfers a block; completion interrupt arrives.

| Method | CPU attention | Best fit | Main cost |
|---|---|---|---|
| Programmed I/O | Every poll/transfer | Simple low-rate device | Busy waiting |
| Interrupt I/O | Per event or chunk | Sporadic events | Interrupt/context overhead |
| DMA | Setup and completion | Large block transfers | Bus contention and cache coordination |

An interrupt handler should be short: save required state, identify/acknowledge source, record work, defer lengthy processing, restore state. Priority/masking prevents one noisy device from monopolizing CPU.

For a 4 MiB transfer, programmed I/O that copies every byte may require millions of CPU-visible operations; DMA can transfer the block while CPU does useful work. Exact transfer speed still depends on bus and device.

Memory-mapped I/O shares address space with memory; a load/store to the device region may have side effects and must not be cached like ordinary RAM. Device registers often require volatile/ordered access rules.

**Review questions**
1. Why can polling be wasteful? CPU spends cycles checking readiness.
2. What does DMA reduce? Per-byte CPU involvement.
3. Why is an interrupt asynchronous? It may arrive independently of current instruction.
4. What could invalidate stale cached data after DMA? Cache coherence/invalidation requirements.
Practice: draw three timelines and annotate when CPU is available for computation.