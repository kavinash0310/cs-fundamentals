# Chapter 4 — Memory Hierarchy, Storage, and Input/Output

**Course:** Computer Fundamentals and ICT  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–3

## What you will learn

This chapter develops memory hierarchy from registers to storage, compares SRAM and DRAM, calculates cache fields and average access time, explains virtual memory and storage devices, and distinguishes programmed I/O, interrupts, and DMA.

---

## 4.1 Why computers use a memory hierarchy

Fast memory is expensive per bit and limited in capacity. Large storage is cheaper per bit but slower. A hierarchy places a small fast level near the CPU and larger slower levels farther away.

~~~mermaid
flowchart TD
    R[Registers: smallest and fastest] --> L1[L1 cache]
    L1 --> L2[L2 and shared cache]
    L2 --> RAM[Main memory]
    RAM --> SSD[SSD or HDD]
    SSD --> ARCH[Archive and backup]
~~~

Typical hierarchy:

1. **Registers** hold immediate operands and CPU state.
2. **Cache** keeps recently or frequently used blocks near the CPU.
3. **Main memory (RAM)** holds active processes and data.
4. **Secondary storage** holds programs and files persistently.
5. **Archive/backup** preserves copies for long-term retention or recovery.

Capacity generally increases down the hierarchy while access speed generally decreases. Exact products and technologies vary.

### Locality

Caches work because programs often show locality:

- **Temporal locality:** recently used data may be used again soon.
- **Spatial locality:** nearby addresses may be used soon.
- **Sequential locality:** instructions are often executed in sequence.

**Example:** A loop that scans an array accesses neighboring elements, so a cache block containing one element may also bring nearby elements into cache.

---

## 4.2 RAM, ROM, SRAM, DRAM, and flash

- **RAM** is memory directly addressable by programs and CPU hardware. Main RAM is usually volatile.
- **DRAM** stores each bit using a capacitor-like cell and needs refreshing; it offers high density and is commonly used for main memory.
- **SRAM** stores bits using a faster multi-transistor circuit and does not need the same refresh process; it is denser in cost and commonly used for cache.
- **ROM** historically means read-only memory. Modern firmware often uses reprogrammable non-volatile flash.
- **Flash memory** retains data without power and is used in SSDs, memory cards, and USB storage.

A **memory cell** stores a bit. A group of bits addressed as one unit is a **memory word**; word size depends on the system.

### Volatile versus non-volatile

- Volatile memory loses its contents when power is removed.
- Non-volatile memory retains data without power.
- Data in volatile RAM can be written many times while powered; volatility does not mean the memory is read-only.

---

## 4.3 Cache terminology and mapping

A cache is divided into **lines** (also called blocks). Each line stores a block of memory plus metadata such as a tag and valid bit.

- **Hit:** requested data is present in the cache.
- **Miss:** requested data is absent, so a lower level must supply it.
- **Hit rate:** fraction of accesses that hit.
- **Miss rate:** 1 − hit rate.
- **Miss penalty:** extra time to obtain a missing block and update the cache.

### Direct-mapped cache

Each main-memory block maps to exactly one cache line.

~~~text
cache line index = memory block number mod number of cache lines
~~~

An address is divided conceptually into:

~~~text
[tag | line index | byte offset]
~~~

- **Byte offset** selects a byte inside the block.
- **Line index** selects a cache line.
- **Tag** identifies which memory block currently occupies that line.

### Worked address-field example

A byte-addressed machine has 32-bit addresses. Its direct-mapped cache has 16 lines, each 16 bytes.

~~~text
Offset bits = log2(16 bytes) = 4
Index bits  = log2(16 lines) = 4
Tag bits    = 32 − 4 − 4 = 24
Address     = [24-bit tag | 4-bit index | 4-bit offset]
~~~

For address 0x12345ABC:

~~~text
Tag    = 0x12345A
Index  = 0xB
Offset = 0xC
~~~

Hex digits correspond to four-bit groups. The final hex digit C is the byte offset; the preceding B is the line index.

### Set-associative and fully associative caches

- **Direct-mapped:** one possible line for each block; simple but can conflict.
- **k-way set associative:** a block maps to one set and may occupy any of k lines in that set.
- **Fully associative:** a block may occupy any line; flexible but needs more tag comparisons.

A replacement policy chooses which line to evict on a miss, such as least recently used (LRU) or an approximation. The precise policy is implementation-dependent.

### Write policies

- **Write-through:** update the cache and lower memory on every write.
- **Write-back:** update cache first and write the block back when evicted; a dirty bit marks changed data.
- **Write-allocate:** on a write miss, bring the block into cache.
- **No-write-allocate:** on a write miss, update lower memory without allocating a cache line.

---

## 4.4 Average memory access time

For a single cache level:

~~~text
AMAT = hit time + miss rate × miss penalty
~~~

**Worked example:** Hit time = 1 ns, miss rate = 0.05, miss penalty = 40 ns.

~~~text
AMAT = 1 + 0.05×40
     = 1 + 2
     = 3 ns
~~~

For multiple levels, a later-level miss penalty includes the access time to lower levels according to the convention in the question. Write down whether a stated penalty is total or additional before calculating.

### Cache size calculation

A cache has 64 lines with 32 bytes per line.

~~~text
Data capacity = 64 × 32 = 2048 bytes = 2 KiB
~~~

The total physical storage is larger because tags, valid bits, dirty bits, and other metadata are also stored.

---

## 4.5 Secondary storage

| Technology | How it stores data | Strengths and limitations |
|---|---|---|
| HDD | Magnetic rotating platters | Large capacity and low cost; mechanical seek latency |
| SSD | Flash memory | Low access latency, no moving platter; endurance and cost vary |
| Optical disc | Laser-readable marks/layers | Portable distribution or archive; slower and capacity-limited |
| Magnetic tape | Sequential magnetic recording | Cost-effective long-term backup; slow random access |

A file may be stored persistently on an SSD and copied into RAM while an application runs. Saving data to a local drive is not automatically a backup; a useful backup is separate enough to survive the failure affecting the original.

---

## 4.6 Virtual memory and address translation

**Virtual memory** lets each process use a virtual address space. The operating system and memory-management hardware translate virtual addresses to physical memory addresses.

~~~mermaid
flowchart LR
    P[Process uses virtual address] --> T[MMU and page-table translation]
    T -->|page in RAM| F[Physical frame in RAM]
    T -->|page absent| X[Page fault exception]
    X --> OS[OS loads page or reports error]
    OS --> F
~~~

- **Page:** fixed-size block of virtual address space.
- **Frame:** fixed-size block of physical memory that can hold a page.
- **Page table:** maps virtual pages to physical frames and records permissions/status.
- **TLB:** small cache of recent address translations.
- **Page fault:** event when a required page is not currently mapped to a usable physical frame. A page fault can be handled normally by loading data, or can indicate an invalid access.

If memory pressure is high, the operating system may move pages between RAM and storage. Storage is much slower, so excessive paging can make a system feel very slow.

---

## 4.7 Memory protection and sharing

Virtual memory helps isolate processes:

- Each process has its own address-space view.
- Page permissions can restrict reading, writing, or executing.
- Shared pages can allow controlled communication and reuse.
- The MMU checks translations and permissions during access.

The OS and hardware jointly enforce these boundaries. A process should not be able to read arbitrary memory belonging to another process.

---

## 4.8 Input/output methods

### Programmed I/O (polling)

The CPU repeatedly checks a device's status and transfers data itself.

- Simple to implement.
- Can waste processor time while waiting.
- Works for simple devices or short operations.

### Interrupt-driven I/O

A device signals the CPU when it needs service or has completed work.

- CPU can do other work while the device operates.
- An interrupt handler services the event.
- Too many interrupts or slow handlers can add overhead.

### Direct Memory Access (DMA)

A DMA controller transfers a block between a device and memory with limited CPU involvement.

~~~mermaid
sequenceDiagram
    participant CPU
    participant DMA
    participant Device
    participant RAM
    CPU->>DMA: Configure address, length, and direction
    DMA->>Device: Request data transfer
    Device->>DMA: Supply or receive data
    DMA->>RAM: Transfer block
    DMA-->>CPU: Interrupt on completion
~~~

DMA is useful for larger transfers. The CPU still configures the transfer and handles completion; DMA does not mean the CPU is never involved.

---

## 4.9 Interrupt concepts

An **interrupt** requests attention from the CPU.

Simplified handling sequence:

1. Finish or suspend the current instruction at a safe point.
2. Save the necessary state.
3. Identify the interrupt source and select a handler.
4. Run the interrupt service routine.
5. Acknowledge or clear the event.
6. Restore state and resume, or schedule another process.

Interrupts can be prioritized and masked. Interrupt handling details differ by architecture and operating system.

---

## 4.10 Worked practice

**1. Cache tag/index/offset.** A 32-bit system has a 64-line direct-mapped cache with 32-byte blocks.

~~~text
Offset = log2(32) = 5 bits
Index = log2(64) = 6 bits
Tag = 32 − 5 − 6 = 21 bits
~~~

**2. AMAT.** Hit time is 2 ns, hit rate is 90%, miss penalty is 50 ns.

~~~text
Miss rate = 1 − 0.90 = 0.10
AMAT = 2 + 0.10×50 = 7 ns
~~~

**3. Cache size.** 128 lines × 64 bytes per line = 8192 bytes = 8 KiB data capacity.

**4. I/O method.** A device reads a large disk block directly into RAM and notifies the CPU when complete.  
**Answer:** DMA with an interrupt on completion.

---

## 4.11 Questions and answers

1. **Why is there a memory hierarchy?**  
   Fast memory is costly and limited; slower storage is larger and cheaper.

2. **What is temporal locality?**  
   Recently used data or instructions are likely to be used again soon.

3. **What is a cache hit?**  
   The requested block is already present in the cache.

4. **What fields are in a direct-mapped address?**  
   Tag, line index, and block/byte offset.

5. **How many offset bits are needed for a 64-byte block?**  
   log₂64 = 6 bits.

6. **What does a TLB cache?**  
   Recent virtual-to-physical address translations.

7. **What is a page fault?**  
   An exception/event for a page that is not currently available as a valid resident translation.

8. **How does DMA differ from programmed I/O?**  
   DMA transfers a block between a device and memory with less CPU data movement.

9. **What is write-back caching?**  
   Data is updated in cache and written to lower memory later, commonly on eviction.

10. **Does virtual memory make storage as fast as RAM?**  
    No. It extends address-space management but storage access is much slower.

### GATE-style practice with solutions

1. Cache has 128 lines and 8-byte blocks. A byte address is 30 bits. Find index, offset, and tag bits.  
   **Solution:** Index = log₂128 = 7; offset = log₂8 = 3; tag = 30−7−3 = 20 bits.

2. Cache hit time 1 ns, miss rate .02, miss penalty 80 ns. Find AMAT.  
   **Solution:** 1 + .02×80 = 2.6 ns.

3. 4-way set associative cache has 64 total lines and 16-byte blocks. How many sets?  
   **Solution:** 64/4 = 16 sets. Offset bits = 4; set-index bits = 4. Tag bits depend on address width.

4. A 256 KiB cache has 64-byte lines. How many data lines?  
   **Solution:** 256×1024 / 64 = 4096 lines.

5. Explain why a page fault does not always mean a program bug.  
   **Solution:** A valid page may simply be absent from RAM and need loading. An invalid address or permission violation is a different case.

### Multiple-choice check

1. A 32-byte cache block needs how many offset bits? A. 4 B. 5 C. 6 D. 32  
2. The cache hit rate is 0.9. Miss rate is: A. 0.01 B. 0.1 C. 0.9 D. 1.9  
3. Which transfers a device block directly to RAM? A. DMA B. polling only C. compiler D. cache eviction  
4. A TLB speeds up: A. virtual address translation B. arithmetic addition C. disk formatting D. character encoding  
5. Which memory typically needs refresh? A. DRAM B. SRAM C. ROM D. optical disc

**Answers:** 1-B, 2-B, 3-A, 4-A, 5-A.

### Draw-from-memory tasks

- Draw the hierarchy from CPU registers down to archival storage and annotate speed/capacity trends.
- Draw a direct-mapped address split and label tag, line index, and offset.
- Draw the DMA transfer sequence from device to RAM and the completion interrupt.
