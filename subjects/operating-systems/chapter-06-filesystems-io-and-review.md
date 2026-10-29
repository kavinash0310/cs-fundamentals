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
