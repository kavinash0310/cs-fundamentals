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

## Extended lecture: I/O paths and interrupt control

There are three common ways for a CPU to coordinate with a device:
- Programmed I/O: CPU repeatedly checks device status and moves each item. Simple but wastes CPU cycles while waiting.
- Interrupt-driven I/O: device interrupts when service is needed. CPU saves context, runs an interrupt handler, acknowledges the device, and resumes.
- DMA: a DMA controller transfers a block between device and memory; CPU configures address, length, and direction, then is interrupted at completion or error.

~~~text
CPU -- configure channel --> DMA controller
                              |          ^
                              v          |
                         main memory <-> device
CPU continues other work until completion interrupt
~~~

An interrupt is usually asynchronous to the current instruction stream (for example, a timer or device event). An exception is synchronous with instruction execution (for example, divide-by-zero or page fault). Exact terminology can vary by architecture, but distinguish external event from instruction-caused event. On entry, hardware saves enough state to resume safely; software determines the source and performs service.

Memory-mapped I/O assigns device registers addresses in the normal address space. Loads and stores access the device; such accesses may have ordering and side-effect rules, so compilers and CPUs cannot freely treat them as ordinary RAM.

### Worked transfer-size comparison

Suppose a device sends 4 MiB in 4 KiB blocks. Interrupt-per-byte is impractical: 4,194,304 interrupts. Interrupt-per-block means 1,024 interrupts. DMA can transfer each block with one setup and completion event while the CPU does useful work. DMA still consumes memory-bus bandwidth and needs cache-coherence or buffer-management care.

### Review questions

1. Which method minimizes CPU copying for large blocks? DMA.
2. Why can a device status read have side effects? The register may clear an interrupt or acknowledge an event.
3. Is a timer interrupt synchronous with the instruction executing? No; it is an asynchronous external event.
4. What must be considered when DMA writes a buffer cached by the CPU? Coherence, cache maintenance, and correct ownership/ordering.
