# Chapter 1 — System Calls, Processes, and Threads

## 1. What an operating system does
The OS manages CPU, memory, devices, files, and protection. A system call is a controlled request from a user program to the kernel, such as read, write, fork, exec, or wait. A library call may wrap one or more system calls.

## 2. Processes and states
A process is a running program plus its address space and OS-managed resources. Typical states: new, ready, running, blocked/waiting, terminated.

~~~text
new -> ready -> running -> terminated
          ^       |
          |       v
          +---- blocked
~~~

A context switch saves one process's registers/PC and restores another's. It enables sharing but costs time and may disrupt caches.

A process control block stores identifiers, state, registers, scheduling and memory information. Address spaces isolate processes; a process may contain one or more threads.

## 3. Threads and IPC
Threads share code/data/heap of a process but have separate stacks and register state. They reduce communication overhead but require synchronization around shared mutable data.

IPC mechanisms include pipes, message queues, shared memory, and sockets. Shared memory is fast after setup but needs synchronization; message passing provides clearer isolation.

## 4. Questions and practice
**Q1.** What does a context switch save? **A:** Execution state such as PC and registers, plus scheduling state.
**Q2.** Which resources do threads commonly share? **A:** Address space and process resources.
**Q3.** Is a system call the same as a normal function call? **A:** It crosses into privileged kernel handling.
Practice: classify process states during disk I/O; compare shared memory and messages; draw fork/exec/wait lifecycle.

## Notebook work
Draw process states and label the events that cause each transition.


## Process lifecycle and privilege boundary

A system call typically switches from user mode to kernel mode, validates arguments/permissions, performs the service, then returns a result or error. A trap is synchronous with the instruction; a hardware interrupt may arrive asynchronously.

On Unix-like systems, fork creates a child process (often copy-on-write pages); exec replaces the current process image; wait collects child status. After fork, parent and child both continue at the return point with different return values. File descriptors may refer to shared open-file descriptions.

A thread context includes its own PC, registers, and stack, while sharing process address space. This makes communication easy but means unsynchronized writes race.

**Practice:** A process blocks on disk I/O: it leaves the running state, another ready process may run, and completion interrupt makes it ready again. Explain why a blocked process is not consuming CPU and why context-switch time is overhead.

## Address spaces and process creation

A process address space is commonly organized into text/code, global/static data, heap, and stack. The OS and MMU prevent one process from directly reading another process's private memory. Shared-memory IPC maps selected pages into both processes, so synchronization is still required.

~~~text
high address  +-------------------+
              | stack grows down  |
              |                   |
              | free region       |
              | heap grows up     |
              | globals/data      |
low address   | program text      |
              +-------------------+
~~~

fork creates a child with a logically separate address space; copy-on-write lets parent and child initially share physical pages until one writes. exec replaces the calling process image while keeping its process identity and selected resources. wait lets a parent collect completion status and avoid an unreaped child entry.

Mode switch and context switch are different: a system call may change privilege mode and return to the same process; a context switch changes which process/thread runs. A process can switch out without a system call (e.g., timer interrupt).