# Chapter 6 — File Systems, I/O, and Review

## 1. Files and directories
A file is named data plus metadata: size, owner, permissions, timestamps, and block mapping. A directory maps names to file identifiers. File descriptors are per-process handles returned by open; read/write advance offsets according to API semantics.

## 2. Allocation and free space
Contiguous allocation offers sequential access but grows poorly and fragments. Linked allocation grows easily but random access is slow. Indexed allocation stores pointers in index blocks; inodes use direct and indirect pointers. Free space can be tracked by bitmap or free list.

A crash can leave metadata inconsistent; journaling records intended updates so recovery can replay or roll back. Permissions and access-control lists constrain operations.

## 3. I/O and storage
Device drivers provide a common OS interface. Buffering handles speed mismatch; caching retains reusable data; spooling queues work for a device. Disk scheduling policies trade seek time and fairness; SSDs have different physical costs than spinning disks.

## 4. Review questions
**Q1.** File descriptor? **A:** Process-local handle to an open file description.
**Q2.** Paging removes which fragmentation? **A:** External, while internal fragmentation may remain.
**Q3.** What does a TLB cache? **A:** Address translations.
**Q4.** When is a cycle sufficient for resource deadlock? **A:** One instance per resource type.
Practice: compare file allocation; trace page replacement; explain journaling after a crash; classify system call vs library call.

## Notebook work
Sketch inode block pointers and summarize process, scheduler, synchronization, deadlock, and memory flow on one page.


## File allocation and disk scheduling

Contiguous file allocation gives fast sequential and random access but growth is difficult and external fragmentation may occur. Linked allocation grows flexibly but random access follows links. Indexed allocation stores block pointers in an index; inode direct/indirect blocks extend capacity.

Disk scheduling examples: FCFS is fair by arrival; SSTF chooses nearest request but can starve far requests; SCAN moves in one direction servicing requests then reverses, like an elevator; C-SCAN services one direction and wraps to the beginning. For cylinders 0–199, head 50, requests 10,20,80,90, SSTF visits 80,90,20,10 (depending on tie policy); calculate total seek as sum of absolute movements.

I/O scheduling policy affects latency/fairness; SSDs have no mechanical seek, so the same cylinder-distance model does not apply directly.

**Review practice:** Compare direct/linked/indexed allocation; calculate SSTF and SCAN movement with direction/end assumptions stated; explain why journaling helps metadata consistency after crash.

## Disk scheduling and crash-consistent files

For head at cylinder 50 and requests 10,20,80,90, FCFS in that order moves 40+10+60+10=120 cylinders. SSTF chooses 80 (30), then 90 (10), then 20 (70), then 10 (10): total 120 here; other request sets can differ sharply. SCAN depends on initial direction and whether the head reaches the physical end before reversing, so state assumptions.

C-LOOK serves requests in one direction, then jumps from the last request to the first pending request rather than the disk edge. It can give more uniform waits than SSTF but still has policy trade-offs.

A file operation may update data blocks, inode metadata, and directory entries. Journaling orders durable log records before home-location updates; after crash, replay committed journal transactions or discard incomplete ones according to mode. Data journaling and metadata journaling have different guarantees.

**Practice:** Calculate FCFS, SSTF, and SCAN movement for a request list, stating direction and endpoints. Explain why cache buffers and disk scheduling are distinct from filesystem naming/permissions.

## Extended lesson: files, directories, and disk scheduling

A file system maps a name and offset to data blocks plus metadata. A directory maps names to file identifiers; an inode-like structure stores ownership, permissions, size, timestamps, and block pointers. Hard links are additional directory names for the same file identity; symbolic links store a path that must be resolved. Removing one hard link does not necessarily delete data while another link or open reference remains.

### Allocation trade-offs

- Contiguous allocation gives fast sequential and direct access but can fragment and makes file growth difficult.
- Linked allocation grows easily and supports sequential access, but random access follows pointers and can be slow.
- Indexed allocation stores block pointers in an index structure and supports direct access, with metadata overhead.
- Extents group runs of contiguous blocks to balance efficient access and flexible growth.

A journal records intended metadata changes so recovery can replay or discard incomplete operations after a crash. Journaling does not automatically mean file contents are fully protected; mode and filesystem differ.

### Disk scheduling example

Requests are at cylinders 10, 20, 80, 90; head starts at 50.
- FCFS order 50→10→20→80→90 moves 40+10+60+10=120 cylinders.
- SSTF chooses the nearest request; there is a tie between 10 and 80 initially (both 40? Wait from 50, distances are 40 to 10, 30 to 20, 30 to 80, 40 to 90), so choose 20 or 80 under a stated tie rule. If choose 20, then 10, then 80, then 90: movement 30+10+70+10=120. A different request set can produce a different total and can starve far requests.
- SCAN sweeps in one direction servicing requests, then reverses at an endpoint; C-SCAN services in one direction and returns to the beginning, offering more uniform wait under its model.

Always state the direction and whether endpoint travel counts. Disk-scheduling answers vary with those assumptions.

### Review questions

1. Which allocation supports direct lookup more naturally than a linked chain? Indexed allocation.
2. Does deleting a filename always erase the data immediately? No; links/open references and delayed reclamation may keep it allocated.
3. Which policy can starve distant requests? SSTF can.
4. Why journal metadata? To recover a consistent filesystem structure after interrupted updates.
