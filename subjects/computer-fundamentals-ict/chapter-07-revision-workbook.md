# Chapter 7 — Computer Fundamentals and ICT Revision Workbook

**Course:** Computer Fundamentals and ICT  
**Exam alignment:** GATE CS foundations across digital representation/COA basics; BPSC computer-teacher foundations and ICT  
**Prerequisite:** Chapters 1–6

This is an original practice set. It is designed to develop understanding and calculation habits, not to reproduce a past paper. For exam-level depth, continue to the linked Digital Logic, Computer Organization, Operating Systems, and Computer Networks courses.

---

## 7.1 How to solve numerical questions

- Write the base and unit beside every number.
- Show powers or division steps rather than only the final converted number.
- For signed arithmetic, state the bit width and representation.
- For cache questions, find offset first, then index or set, then tag.
- For performance questions, keep units in the calculation.
- For network rate questions, distinguish bits/s from bytes/s.
- For a conceptual question, define the term, state its role, and add a contrast or example.

---

## 7.2 Fully worked numerical questions

### Problem 1: Binary to decimal

Convert 1100101₂ to decimal.

~~~text
1×2^6 + 1×2^5 + 0×2^4 + 0×2^3 + 1×2^2 + 0×2^1 + 1×2^0
= 64 + 32 + 4 + 1
= 101₁₀
~~~

### Problem 2: Decimal to binary

Convert 83₁₀ to binary.

~~~text
83 = 64 + 16 + 2 + 1
Therefore bits for 2^6, 2^4, 2^1, and 2^0 are 1:
83₁₀ = 1010011₂
~~~

### Problem 3: Binary to hexadecimal and octal

~~~text
11010110₂ = 1101 0110₂ = D6₁₆
11010110₂ = 011 010 110₂ = 326₈
~~~

Check: D6₁₆ = 13×16+6 = 214. 326₈ = 3×64+2×8+6 = 214.

### Problem 4: Signed range and overflow

An 8-bit two's-complement number has range −128 through +127. Add +125 and +10:

~~~text
  01111101  (+125)
+ 00001010  (+10)
----------
  10000111
~~~

The mathematical result is +135, which is outside the signed range. The bit pattern 10000111 is interpreted as −121. This is signed overflow, not a valid +135 representation.

### Problem 5: CPU time

A program executes 240 million instructions with average CPI 1.25 on a 1.5 GHz CPU.

~~~text
CPU time = 240×10^6 × 1.25 / 1.5×10^9
         = 300×10^6 / 1.5×10^9
         = 0.2 seconds
~~~

### Problem 6: Cache address fields

A 32-bit byte-addressed system has a 64 KiB, 4-way set-associative cache with 64-byte blocks.

~~~text
Total lines = 64 KiB / 64 B = 1024 lines
Number of sets = 1024 / 4 = 256 sets
Offset bits = log2(64) = 6
Set-index bits = log2(256) = 8
Tag bits = 32 − 6 − 8 = 18
~~~

### Problem 7: Average memory access time

Cache hit time is 2 ns, miss rate is 0.04, and miss penalty is 50 ns.

~~~text
AMAT = hit time + miss rate × miss penalty
     = 2 + 0.04×50
     = 4 ns
~~~

### Problem 8: Ideal pipeline cycles

A five-stage pipeline executes 12 instructions without stalls.

~~~text
Cycles = stages + instructions − 1
       = 5 + 12 − 1
       = 16 cycles
~~~

This ideal formula assumes one cycle per stage and no hazards or pipeline overhead.

### Problem 9: Image storage

A raw 1280×720 image uses 24 bits per pixel.

~~~text
Bits = 1280 × 720 × 24
     = 22,118,400 bits
Bytes = 22,118,400 / 8
      = 2,764,800 bytes ≈ 2.765 MB decimal
~~~

### Problem 10: Network throughput units

An 80 Mb/s link has ideal data capacity:

~~~text
80 megabits per second / 8 = 10 megabytes per second
~~~

Actual file transfer is lower because of protocol overhead, congestion, server speed, and storage limits.

---

## 7.3 Short-answer review

1. **Why is data representation important?**  
   A bit pattern has no unique meaning until a format defines whether it is a number, character, instruction, or media sample.

2. **How does a CPU differ from a complete computer system?**  
   The CPU executes instructions; a complete system also requires memory, storage, I/O, software, and supporting components.

3. **What is the difference between RAM and an SSD?**  
   RAM is working memory, normally volatile; an SSD is persistent secondary storage.

4. **What is a cache hit?**  
   The requested data block is present in the cache.

5. **What is the role of the tag in a cache address?**  
   It identifies which memory block is stored in the selected line or set.

6. **How do an interrupt and DMA differ?**  
   An interrupt requests CPU attention; DMA transfers a block between a device and memory with reduced CPU data movement.

7. **Does a page fault always mean invalid memory access?**  
   No. A valid page may simply need to be brought into RAM.

8. **How does an OS differ from an application?**  
   The OS manages resources and provides services; an application performs a user task.

9. **How does the Internet differ from the Web?**  
   The Internet is network infrastructure; the Web is a linked-resource service running over it.

10. **Why is HTTPS not proof that a website is trustworthy?**  
    It protects the connection to the named domain but does not verify that the site's claims are honest or safe.

---

## 7.4 Multiple-choice practice

Choose one answer.

1. Which register usually contains the address of the next instruction?  
   A. PC  B. MAR  C. MDR  D. ALU

2. Which unit controls instruction sequencing?  
   A. Control unit  B. Printer  C. SSD  D. Scanner

3. What is 45₁₀ in hexadecimal?  
   A. 2D  B. 35  C. 55  D. 101101

4. An 8-bit unsigned value can represent:  
   A. 0 to 127  B. −128 to 127  C. 0 to 255  D. −255 to 255

5. The 8-bit two's-complement value 11111111₂ represents:  
   A. 255  B. −1  C. −127  D. 0

6. Which character standard assigns code points across writing systems?  
   A. Unicode  B. BCD  C. DMA  D. ASCII only

7. Which memory commonly requires refresh?  
   A. DRAM  B. SRAM  C. ROM  D. SSD

8. What does temporal locality mean?  
   A. Recently used data may be used again soon  
   B. Nearby addresses are never used  
   C. Data is stored permanently  
   D. A processor has two clocks

9. A 128-byte cache block needs how many offset bits?  
   A. 6  B. 7  C. 8  D. 128

10. A page table maps:  
    A. virtual pages to physical frames  
    B. file names to extensions  
    C. CPU registers to ALU operations  
    D. DNS names to URLs only

11. A DMA controller primarily:  
    A. transfers data between a device and memory  
    B. compiles source code  
    C. encrypts every web page  
    D. schedules all CPU instructions

12. Which device forwards packets between IP networks?  
    A. Router  B. Keyboard  C. Monitor  D. Scanner

13. DNS commonly returns:  
    A. records such as IP addresses for names  
    B. the complete web page  
    C. CPU instructions  
    D. video frames

14. HTTPS protects HTTP communication using:  
    A. TLS  B. BCD  C. OCR  D. DMA

15. Which is a system-software example?  
    A. Operating system  B. Student photograph  C. Spreadsheet document  D. Printer paper

16. Which is not an operating-system responsibility?  
    A. Process management  B. Memory management  C. File management  D. Deciding truth of every input

17. If hit time is 1 ns, miss rate .1, and miss penalty 20 ns, AMAT is:  
    A. 1 ns  B. 2 ns  C. 3 ns  D. 20 ns

18. A 4-way cache has 128 lines total. Number of sets is:  
    A. 16  B. 32  C. 128  D. 512

19. Which security goal means information is changed only in authorised ways?  
    A. Integrity  B. Availability  C. Confidentiality  D. Throughput

20. A backup should be:  
    A. a separate recoverable copy  
    B. a cache line  
    C. a process register  
    D. a file extension

### Answer key with explanations

1. A — PC identifies the next instruction in the simplified CPU model.  
2. A — control unit sequences operations.  
3. A — 45 = 2×16 + 13, and 13 is D.  
4. C — unsigned 8-bit range is 0 through 2⁸−1.  
5. B — all ones is −1 in two's complement.  
6. A — Unicode assigns code points; encodings such as UTF-8 store them as bytes.  
7. A — DRAM cells require periodic refresh.  
8. A — temporal locality is reuse over time.  
9. B — log₂128 = 7.  
10. A — virtual page numbers map to physical frames.  
11. A — DMA moves blocks with reduced CPU data copying.  
12. A — routers forward packets between IP networks.  
13. A — DNS is a distributed naming system.  
14. A — HTTPS uses TLS.  
15. A — an operating system manages resources and application services.  
16. D — a computer system processes according to rules; correctness of input meaning requires people and specification.  
17. C — AMAT = 1 + .1×20 = 3 ns.  
18. B — sets = 128/4 = 32.  
19. A — integrity concerns correctness and authorised changes.  
20. A — a backup is for recovery and should be separable from the original failure.

---

## 7.5 Teaching and exam-writing prompts

1. Draw a labelled computer block diagram and explain each path using a school attendance example.
2. Compare RAM, cache, and secondary storage using speed, capacity, volatility, and purpose.
3. Explain binary, octal, decimal, and hexadecimal conversion with one integer and one fractional example.
4. Explain how a browser finds a web server and receives a page. Include DNS, IP, HTTPS, HTML, and CSS.
5. Explain why ICT can improve feedback but still requires a teacher to check accessibility, privacy, and fairness.
6. Draw the virtual-memory translation path and distinguish a valid page fault from an invalid access.
7. Explain the CIA triad using examples from student records.
8. A computer has a fast CPU but slow response when reading a remote file. Name and explain three possible bottlenecks.

### Model outline for prompt 8

Possible causes include network latency or congestion, a slow server/storage device, insufficient RAM causing paging, file-system delays, or CPU processing after data arrives. Measure the resource involved and reproduce the problem; do not infer the CPU is the only bottleneck from its clock rate.

---

## 7.6 Final self-assessment

Before moving on, make sure you can:

- convert integers and binary fractions between bases;
- explain signed representation and detect overflow;
- calculate CPU time, cache fields, AMAT, and ideal pipeline cycles;
- distinguish RAM, cache, virtual memory, and storage;
- describe programmed I/O, interrupt-driven I/O, and DMA;
- explain OS services, DNS, HTTPS, and the CIA triad;
- redraw the computer, cache, boot, and browser-request diagrams without looking.

**Next courses for full GATE coverage:** Digital Logic, Computer Organization and Architecture, Operating Systems, and Computer Networks. These expand the introductory links in this course into full subject depth.
