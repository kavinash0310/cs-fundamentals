# Chapter 4 — Memory Hierarchy and Cache Mapping

## 1. Locality
Temporal locality: recently used data may be used again. Spatial locality: nearby addresses may be used. Cache stores recently used blocks close to CPU; main memory is larger but slower; secondary storage is larger still and slower.

Average memory access time for one cache level:
~~~text
AMAT = hit time + miss rate × miss penalty
~~~
Example: hit time 1 ns, miss rate 0.04, penalty 50 ns gives AMAT=3 ns.

## 2. Address fields
For byte-addressed memory with block size 2^b bytes, low b bits are block offset. A direct-mapped cache with 2^s lines uses s index bits; remaining high bits are tag. Number of data bytes = lines×block size.

Example: 32-bit address, 64-byte block (b=6), 256 lines (s=8): tag bits =32-6-8=18.

## 3. Mapping
Direct-mapped: each memory block has one cache line; simple, conflict misses possible.
Fully associative: any block can occupy any line; needs associative search and replacement.
Set associative: cache is partitioned into sets; a block maps to one set and may occupy any way within it.

On miss, a replacement policy such as LRU, FIFO, or random chooses a victim when required. Write-through updates memory on every write; write-back updates a dirty cache line when evicted. Write-allocate loads a missing block on write; no-write-allocate writes around cache.

## 4. Miss types
Compulsory: first access. Capacity: cache too small for working set. Conflict: mapping collisions. These categories explain why increasing associativity can help conflicts but not all misses.

## 5. Questions and practice
**Q1.** Offset bits for 32-byte blocks? **A:** 5.
**Q2.** A 4-way set associative cache with 64 lines has how many sets? **A:** 16.
**Q3.** What is a dirty bit? **A:** Indicates a modified line must be written back.
**Q4.** Compute AMAT for hit 2 ns, miss rate .1, penalty 30 ns. **A:** 5 ns.
Practice: split an address into tag/index/offset; compare two mapping policies; explain locality for a matrix row-major traversal.

## Notebook work
Draw direct-mapped and 2-way set associative cache layouts and mark how the same address maps.
