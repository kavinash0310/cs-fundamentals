# Chapter 5 — Memory Management and Virtual Memory

## 1. Address translation
A process uses virtual addresses; hardware and OS map them to physical memory. Base/limit registers provide simple relocation/protection. Paging divides virtual memory into pages and physical memory into frames of equal size.

Virtual address = page number + offset. Page table maps page→frame; physical address = frame number + same offset. A TLB caches recent translations. On a TLB miss, page-table walk occurs; if page is absent, page fault triggers OS loading or invalid access handling.

## 2. Paging costs
For page size 2^k, low k address bits are offset. Larger pages reduce page-table size and TLB pressure but can increase internal fragmentation. Multilevel page tables allocate structures only for used regions. Inverted tables scale with physical frames.

Effective access time depends on TLB hit rate and memory lookup costs; calculate carefully from the stated model.

## 3. Virtual memory and replacement
Demand paging loads pages on first use. Locality creates working sets; if active pages exceed memory, thrashing occurs.

Replacement policies:
- FIFO: oldest loaded; may exhibit Belady's anomaly.
- LRU: least recently used; stack property, but exact implementation cost.
- Optimal: evict page used farthest in future; theoretical lower bound, not implementable online.
- Clock/second chance: approximate recency with reference bits.

## 4. Allocation and fragmentation
Contiguous allocation suffers external fragmentation; paging removes external fragmentation but has internal waste in final page. Segmentation matches logical variable-sized regions but can fragment. Swapping moves pages/processes between memory and storage.

## 5. Questions and practice
**Q1.** What stays unchanged during page translation? **A:** Page offset.
**Q2.** What causes a page fault? **A:** Referenced page not currently resident or invalid mapping.
**Q3.** Which replacement is optimal in hindsight? **A:** Belady's optimal.
Practice: split an address; trace FIFO/LRU; compare page sizes; calculate TLB effective access from a hit ratio.

## Notebook work
Draw virtual-to-physical translation through TLB and page table, including a page fault branch.


## Address translation and page-replacement trace

Page size 1 KiB = 2¹⁰ bytes, so offset uses 10 bits. Virtual address 5000: page number=floor(5000/1024)=4, offset=904. If page 4 maps to frame 9, physical address=9×1024+904=10120.

For 32-bit virtual addresses and 4 KiB pages, offset=12 bits and page number=20 bits. A TLB hit avoids a page-table memory access; a miss may require one or multiple memory accesses, depending on page-table structure.

LRU with 3 frames, reference string 1,2,3,1,4: after 1,2,3, hit 1 updates recency; loading 4 evicts 2, the least recently used. FIFO instead evicts 1, the oldest-loaded. Track recency/order after each reference.

A page fault may be valid demand paging or invalid access. Valid fault: trap to OS, locate page, choose/evict frame if needed, read storage, update page table/TLB, restart instruction. Dirty victim must be written back.

**Practice:** Trace FIFO and LRU for 1,2,3,1,4,2; calculate page offset bits for 8 KiB; explain thrashing using working set.