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
