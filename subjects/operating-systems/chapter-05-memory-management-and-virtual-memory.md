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
