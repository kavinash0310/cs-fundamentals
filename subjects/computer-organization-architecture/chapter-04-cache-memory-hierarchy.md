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


## 6. Address-field worked examples

A 32-bit byte address, 4 KiB direct-mapped cache, 16-byte blocks:
- Number of lines = 4096/16 = 256 = 2⁸, so index is 8 bits.
- Offset = log₂16 = 4 bits.
- Tag = 32-8-4 = 20 bits.
- Address fields are tag[31:12], index[11:4], offset[3:0].

For a 16 KiB cache, 4-way, 64-byte blocks: sets=(16384/64)/4=64; offset=6 bits, set-index=6 bits.

A cache line contains a valid bit, tag, data block, and often dirty/replacement state. On a read hit, return requested bytes selected by offset. On a miss, fetch the containing block, choose a line/way, update tag/valid, then return data.

Write-back example: CPU writes a cached block, setting dirty=1. On eviction, memory receives the modified block. Write-through sends each write to memory immediately; a write buffer can hide some latency.

AMAT with L1 hit=1 ns, L1 miss=.05, L2 hit=8 ns, L2 miss=.10, memory penalty=80 ns:
~~~text
AMAT = 1 + .05 * (8 + .10*80) = 1.8 ns
~~~
The L2 time is paid only on L1 miss; memory penalty only on L2 miss.

**Practice with answers**
1. 64-byte block means 6 offset bits.
2. 8-way cache with 512 lines has 64 sets.
3. Matrix row traversal often has spatial locality; column traversal can waste cache lines in row-major storage.
4. Increasing associativity can reduce conflict misses but adds lookup complexity.