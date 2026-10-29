# Chapter 5 — Software, Operating Systems, and Files

**Course:** Computer Fundamentals and ICT  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–4

## What you will learn

You will classify software, explain the path from source code to execution, describe core operating-system services, understand processes and files at an introductory level, and trace a typical boot sequence.

---

## 5.1 Software categories

- **System software:** manages resources and provides common services. Examples include operating systems, device drivers, and language runtimes.
- **Application software:** performs a user task. Examples include a browser, word processor, result portal, or spreadsheet.
- **Utility software:** performs maintenance or protection tasks such as backup, compression, and malware scanning.
- **Firmware:** low-level software stored in non-volatile memory that starts or controls a device.
- **Middleware:** software connecting applications or services, for example a message broker or application server.

Software needs hardware resources to run. Hardware may have limited capability without suitable system software.

---

## 5.2 Source code to running program

~~~mermaid
flowchart LR
    SRC[Source code] --> PRE[Preprocessor, when used]
    PRE --> COMP[Compiler]
    COMP --> ASM[Assembler or object generation]
    ASM --> LINK[Linker and libraries]
    LINK --> EXE[Executable]
    EXE --> LOAD[Loader]
    LOAD --> RAM[Memory]
    RAM --> CPU[Processor executes]
~~~

The exact stages depend on the language and toolchain.

- **Preprocessor:** expands directives such as C header inclusion and macros.
- **Compiler:** translates source code into another form, such as machine code or intermediate code.
- **Assembler:** translates assembly instructions to machine code.
- **Linker:** combines object files and libraries and resolves references.
- **Loader:** places an executable in memory and prepares it to run.
- **Interpreter/runtime:** executes instructions or intermediate code. Many practical systems combine interpretation and compilation.
- **JIT compiler:** compiles some code at runtime.

Do not assume that a language is always either compiled or interpreted. Python implementations commonly compile to bytecode and execute it in a virtual machine; other implementation strategies exist.

---

## 5.3 Operating-system definition and services

An **operating system (OS)** manages computer resources and provides services to applications and users.

~~~mermaid
flowchart TD
    U[Users] --> APP[Applications]
    APP --> OS[Operating system]
    OS --> CPU[CPU and processes]
    OS --> MEM[Memory]
    OS --> FS[Files and storage]
    OS --> DEV[Devices]
    OS --> NET[Network interfaces]
~~~

Main OS responsibilities:

1. **Process management:** creates, schedules, and terminates processes.
2. **Memory management:** allocates memory and protects address spaces.
3. **File management:** organises files and directories.
4. **Device management:** controls hardware through device drivers.
5. **Security:** authenticates users and checks permissions.
6. **Networking:** provides communication services.
7. **User interface:** offers a GUI, CLI, or both.
8. **Error handling and accounting:** records events and resource use.

Detailed scheduling, synchronization, deadlocks, memory management, and file-system algorithms are taught in the Operating Systems course.

---

## 5.4 Program, process, and thread

- A **program** is stored instructions and data, typically in a file.
- A **process** is a running program with its own execution state and resources.
- A **thread** is an execution path within a process. Threads in one process typically share much of that process's memory.
- The **scheduler** chooses which runnable process or thread gets CPU time.
- A **context switch** saves one execution state and restores another.

A browser application can have multiple processes and threads. The exact design depends on the OS and application.

### Process-state sketch

~~~mermaid
stateDiagram-v2
    [*] --> New
    New --> Ready
    Ready --> Running
    Running --> Waiting
    Waiting --> Ready
    Running --> Ready
    Running --> Terminated
    Terminated --> [*]
~~~

The diagram is a basic model: New, Ready, Running, Waiting/Blocked, and Terminated. A process may wait for keyboard input, disk I/O, a network reply, or another event.

---

## 5.5 User interfaces and command execution

- **GUI:** graphical windows, icons, menus, and pointer/touch interaction.
- **CLI:** typed commands interpreted by a shell.
- **Shell:** program that accepts commands and launches programs; a shell is not the same as the operating-system kernel.
- **System call:** controlled request by a program for a service such as opening a file or creating a process.
- **Device driver:** software that mediates between the OS and a specific device class.

A command-line program can be powerful and scriptable, but users must understand command syntax and paths.

---

## 5.6 Files, directories, and file systems

A **file system** organises persistent data into named files and directories.

- **File:** named data, such as a document or image.
- **Directory/folder:** container for files and child directories.
- **Path:** identifies a file's location.
- **Absolute path:** starts from the file-system root or drive root.
- **Relative path:** is interpreted relative to the current working directory.
- **Extension:** suffix such as .txt or .pdf; it suggests a format but does not prove the file's actual content.
- **Metadata:** information about a file, such as its size, timestamps, owner, and permissions.
- **Permissions:** rules about which users may read, write, or execute an item.

~~~text
/ (root)
└── home
    └── learner
        ├── notes
        │   └── computer.txt
        └── programs
            └── result.py
~~~

If the current directory is /home/learner, the relative path notes/computer.txt identifies the same file as the absolute path /home/learner/notes/computer.txt.

### File operations

Common operations include create, open, read, write, append, rename, copy, move, and delete. A program should check for errors such as missing files, insufficient permission, or a full disk.

Deleting a file may remove its directory entry without immediately overwriting every physical bit. Secure deletion and recovery depend on the file system and storage technology.

### Backups

A backup is a separate recoverable copy. Keep a backup isolated enough that one failure or malware event cannot destroy both the working copy and its backup. Test that important backups can actually be restored.

---

## 5.7 Boot process

**Booting** loads the operating system and prepares the computer for use.

~~~mermaid
flowchart LR
    P[Power on] --> F[Firmware: UEFI or BIOS]
    F --> C[Hardware checks and setup]
    C --> B[Select boot device]
    B --> L[Bootloader]
    L --> K[Load OS kernel]
    K --> S[Start drivers and services]
    S --> U[Login and user interface]
~~~

Modern personal computers commonly use UEFI; BIOS is also used as a historical and general term. The exact process differs across PCs, phones, servers, and embedded devices.

A **cold boot** starts after power was off. A **warm boot/restart** restarts without a full power-off. Sleep and hibernation are different power states.

---

## 5.8 Software licensing and maintenance

- **Proprietary software:** use and modification are restricted by a licence.
- **Open-source software:** source is available under a licence that grants specified rights; obligations vary by licence.
- **Freeware:** generally available at no monetary cost, but may still be proprietary.
- **Shareware/trial software:** distributed for evaluation or limited use.
- **Update:** changes software to fix issues or add features.
- **Security patch:** update addressing a security weakness.

“Open source” does not mean “no licence” or “no obligations”. Follow the terms of the actual licence.

---

## 5.9 Worked examples

**1. Classify the software.**

- Linux kernel: system software.
- A spreadsheet application: application software.
- A printer driver: system software.
- Backup utility: utility software.
- Code in router firmware: firmware.

**2. A program cannot save a file. List a diagnostic sequence.**

1. Confirm the intended path and file name.
2. Check whether the directory exists.
3. Check write permission and available space.
4. Inspect the application's error message or return code.
5. Try a small file in a known writable location.
6. Avoid rerunning a destructive overwrite until the existing data is backed up.

**3. A program is stored on disk but has not run. Is it a process?**  
No. The program is stored code; a process is a running instance with execution state and resources.

---

## 5.10 Questions and answers

1. **What is system software?**  
   Software that manages resources or provides a platform for applications.

2. **What does a linker do?**  
   Combines object files and libraries and resolves references among them.

3. **How does a program differ from a process?**  
   A program is stored code; a process is code in execution with state and resources.

4. **What is a system call?**  
   A controlled request from a program for an operating-system service.

5. **What is a shell?**  
   A command interpreter that accepts commands and launches programs.

6. **What is a file system?**  
   A method for organising, naming, retrieving, and managing files on storage.

7. **What is an absolute path?**  
   A path interpreted from the root or drive root.

8. **What happens during booting?**  
   Firmware initialises hardware, selects a boot device, loads the OS, and starts services and a user interface.

9. **Why are backups important?**  
   They allow recovery after deletion, corruption, hardware failure, or attack.

10. **Does open source mean there are no restrictions?**  
    No. The actual open-source licence specifies permissions and obligations.

### Multiple-choice check

1. A running instance of a program is a: A. process B. directory C. firmware image D. source file  
2. Which combines object files and libraries? A. linker B. loader C. shell D. scanner  
3. Which starts before the operating-system kernel? A. firmware B. browser tab C. user file D. printer queue  
4. A shell is best described as: A. command interpreter B. CPU register C. cache line D. file-system block  
5. Which is a separate recovery copy? A. backup B. shortcut C. cache D. process

**Answers:** 1-A, 2-A, 3-A, 4-A, 5-A.

### Draw-from-memory task

Draw user → application → OS → CPU/memory/files/devices. Then sketch booting from power-on through firmware, bootloader, kernel, and login.
