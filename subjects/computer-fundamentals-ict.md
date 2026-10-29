# Computer Fundamentals and ICT — Notebook 01

> **How to use this note:** Copy the boxed definitions, diagrams, and examples into a notebook in your own handwriting. Read the explanation first, then cover it and redraw the diagram from memory. This is a full foundation chapter for computer-teacher preparation and basic technical interviews; compare exam-specific facts with the latest official notification for your target post.

## Learning checklist

After this chapter, you should be able to:

- Explain data, information, the IPO cycle, and the limits of computers.
- Draw the functional units of a computer and describe the CPU instruction cycle.
- Compare primary memory, secondary storage, and common input/output devices.
- Convert numbers among decimal, binary, octal, and hexadecimal and solve basic binary arithmetic.
- Explain character encoding, operating systems, software translation, booting, networks, web services, and cybersecurity.
- Answer recall, calculation, teaching, and interview-style questions.

---

## 1. Computer, data, and information

### Definition to write

A **computer** is an electronic, programmable machine that accepts data and instructions, processes them according to a program, produces information, and can store data and results for later use.

### The IPO cycle

A computer system can be understood through four basic operations:

~~~mermaid
flowchart LR
    I[Input: raw data and instructions] --> P[Processing: operations by CPU]
    P --> O[Output: useful results]
    P <--> S[Storage: data and programs]
~~~

**Example — student result:** Marks entered for each subject are input data. The computer totals marks and calculates a percentage. The displayed result is information. The record may be saved in storage.

- **Data:** raw facts, symbols, measurements, or observations. Example: 72, 81, 66.
- **Information:** data organised or processed so that it answers a question. Example: total = 219 and average = 73.
- **Instruction:** an operation a computer is asked to perform.
- **Program:** an ordered set of instructions that solves a task.
- **Information and Communication Technology (ICT):** technologies used to create, process, store, communicate, and share information. ICT includes computers, phones, networks, software, radio, and digital learning tools.

### Main characteristics

- **Speed:** electronic operations can be completed very quickly.
- **Accuracy:** results are accurate when the data, program, and hardware are correct. Incorrect input can still produce an incorrect result.
- **Storage:** large amounts of data can be kept and retrieved.
- **Automation:** after correct instructions and input are supplied, repeated work can run automatically.
- **Diligence:** a computer can repeat a task without human fatigue, although hardware can fail.
- **Versatility:** the same system can support writing, calculation, communication, design, and many other tasks.
- **Connectivity:** networked computers can exchange data and share services.

**Limitations:** A computer does not automatically understand whether an instruction is sensible, ethical, or true. It depends on people for goals, correctly designed programs, reliable data, and maintenance. Artificial-intelligence systems can generate useful outputs but can also make errors.

**Exam distinction:** “Garbage in, garbage out” means poor or misleading input and rules can produce poor or misleading output.

---

## 2. Types and generations of computers

### Types by size and use

| Type | Typical role | Example or note |
|---|---|---|
| Supercomputer | Very large scientific calculations and simulations | Weather modelling |
| Mainframe | High-volume, reliable transaction processing for many users | Banking or reservation systems |
| Server | Provides data or services to other computers over a network | Web or database server |
| Workstation | Powerful single-user professional work | Engineering or graphics |
| Personal computer | General-purpose use by one person | Desktop or laptop |
| Mobile/embedded computer | Portable or built into another device | Smartphone, router, vehicle controller |

These categories describe common uses, not a strict ranking. A server can be physically small, and a powerful desktop may outperform an older system called a mainframe in some tasks.

### Computer generations — common school-level classification

| Generation | Main technology | Commonly associated development |
|---|---|---|
| First | Vacuum tubes | Early electronic computers; large and power-hungry |
| Second | Transistors | Smaller and more reliable than vacuum-tube systems |
| Third | Integrated circuits | Many electronic components placed on a chip |
| Fourth | Microprocessors using VLSI technology | Personal computers and modern digital devices |
| Fifth | Often described in textbooks through AI and advanced processing | This label is not a universally precise hardware boundary |

**Remember:** V-T-I-M is a useful technology sequence: **V**acuum tube → **T**ransistor → **I**ntegrated circuit → **M**icroprocessor. Dates and fifth-generation descriptions can vary by textbook.

---

## 3. Functional units and CPU

### Block diagram to copy

~~~mermaid
flowchart LR
    IN[Input unit] --> MEM[Main memory]
    MEM <--> CPU[CPU]
    CPU --> OUT[Output unit]
    SEC[Secondary storage] <--> MEM
    subgraph CPU
      ALU[ALU: arithmetic and logic]
      CU[Control unit: directs operations]
      REG[Registers: fastest small storage]
    end
~~~

- **Input unit:** accepts data and instructions and converts them to a form the system can process.
- **Central Processing Unit (CPU):** executes instructions and coordinates system operations. It contains the arithmetic logic unit, control unit, and registers.
- **Arithmetic Logic Unit (ALU):** performs arithmetic such as addition and comparisons such as greater than or equal to.
- **Control Unit (CU):** fetches and decodes instructions and sends control signals to coordinate execution. It directs work; it is not the part that performs all arithmetic itself.
- **Registers:** very small, very fast storage locations inside the CPU. They hold values such as the current instruction, an address, or an intermediate result.
- **Output unit:** presents results in a usable form, such as text on a screen or sound through a speaker.
- **Main memory:** holds the programs and data currently being used.

### Fetch–decode–execute cycle

1. **Fetch:** the CPU obtains the next instruction from memory.
2. **Decode:** the control unit determines what the instruction means and what operands it needs.
3. **Execute:** the CPU performs the operation.
4. **Store or write back:** a result may be written to a register or memory.
5. The cycle continues, unless the program halts or control moves elsewhere.

~~~mermaid
flowchart LR
    A[Fetch instruction] --> B[Decode instruction]
    B --> C[Execute operation]
    C --> D[Store result if needed]
    D --> A
~~~

The CPU uses a **clock** to coordinate steps. Clock frequency is measured in hertz. A higher clock frequency alone does not guarantee a faster computer: processor design, number of cores, memory, workload, and software also matter.

### Von Neumann and Harvard architectures

- In the **Von Neumann model**, instructions and data share the same main memory and commonly share paths to the processor. This simple design can create a bottleneck when instructions and data compete for access.
- In the **Harvard model**, instructions and data have separate memories or paths. This can allow simultaneous access in some designs.
- Modern processors often combine ideas from both models, such as separate instruction and data caches.

---

## 4. Input, output, and input/output devices

### Input devices

- **Keyboard:** enters text and commands.
- **Mouse or touchpad:** points, selects, and moves objects.
- **Touchscreen:** both displays output and senses touch input.
- **Scanner:** converts a paper image into a digital image.
- **Microphone:** converts sound into an electrical/digital signal.
- **Webcam:** captures still images or video.
- **Barcode/QR reader:** reads encoded patterns.
- **Biometric reader:** measures a feature such as a fingerprint.

### Output devices

- **Monitor/display:** visual output.
- **Printer:** paper output; inkjet and laser are common types.
- **Speaker/headphones:** audio output.
- **Projector:** displays an image on a larger surface.
- **Plotter:** produces large or precise drawings, often for design work.

### Devices that can do both

A touchscreen, storage drive, network adapter, and multifunction printer can perform input and output roles depending on the task.

**Scanner versus OCR:** A scanner captures the page as pixels. **Optical Character Recognition (OCR)** analyses those pixels to identify character shapes and produce editable text. A scanned page is not automatically searchable text unless OCR or an equivalent text layer is present.

---

## 5. Memory and storage

Memory and storage both retain data, but they serve different purposes. **Main memory** gives the CPU fast access to current work. **Secondary storage** keeps files and programs when power is off.

### Memory hierarchy

~~~mermaid
flowchart TD
    R[CPU registers: smallest and fastest] --> C[Cache]
    C --> M[RAM: working memory]
    M --> S[SSD or HDD: persistent storage]
    S --> A[Archive or backup: long-term copy]
~~~

Moving down the hierarchy usually increases capacity and reduces cost per bit, but access becomes slower. Exact speed and capacity depend on the device.

| Component | Volatile? | Typical purpose |
|---|---|---|
| Registers | Yes | Immediate CPU operands and control values |
| Cache | Yes | Keeps frequently used instructions/data close to CPU |
| RAM | Yes | Holds active programs and data |
| ROM/firmware storage | Usually non-volatile | Keeps startup or device firmware |
| SSD/HDD | No | Persistent operating system, applications, and files |
| Optical media/tape | No | Distribution, archival, or backup use |

- **Volatile memory** loses its contents when power is removed. RAM is normally volatile.
- **Non-volatile storage** keeps information without power.
- **RAM (Random Access Memory):** working memory accessible by address. “Random access” means locations can be reached directly; it does not mean the data is random.
- **DRAM:** commonly used as main memory; it must be refreshed periodically.
- **SRAM:** faster and more expensive per bit than DRAM; commonly used for cache.
- **ROM (Read-Only Memory):** a traditional name for non-volatile memory used for fixed or rarely changed data. Modern firmware may be stored in reprogrammable flash.
- **Cache:** small, fast memory that stores copies of data likely to be used soon.
- **Virtual memory:** operating-system technique that uses storage to extend the address space available to programs. It is much slower than RAM and is not a replacement for physical RAM.
- **SSD:** uses flash memory and has no moving disk platter.
- **HDD:** stores data magnetically on rotating platters and uses moving parts.
- **Backup:** a separate copy used for recovery. A second folder on the same failing drive is not a reliable backup.

**Common interview comparison:** RAM is temporary working space; an SSD/HDD is persistent file storage. A computer can have free storage but still run slowly if it lacks enough RAM for active programs.

---

## 6. Bits, bytes, and data units

- A **bit** is a binary digit: 0 or 1.
- A **nibble** is 4 bits.
- A **byte** is 8 bits.
- A **word** is the natural data size handled by a processor design; its size depends on the architecture.

### Decimal and binary units

| Unit | Decimal convention | Binary convention |
|---|---:|---:|
| kilobyte / kibibyte | 1 kB = 1,000 bytes | 1 KiB = 1,024 bytes |
| megabyte / mebibyte | 1 MB = 1,000,000 bytes | 1 MiB = 1,048,576 bytes |
| gigabyte / gibibyte | 1 GB = 1,000,000,000 bytes | 1 GiB = 1,073,741,824 bytes |

Storage manufacturers commonly use decimal units. Some operating systems display binary-sized quantities with familiar labels, so a displayed capacity can seem different. Use the unit convention stated in a question.

**Network speeds:** b = bit; B = byte. Because 1 byte = 8 bits, 80 megabits per second is at most 10 megabytes per second before protocol overhead.

**Worked example:** A file has 24,000,000 bits. In bytes: 24,000,000 ÷ 8 = 3,000,000 bytes = 3 MB using decimal units.

---

## 7. Number systems and conversion

A positional number system has a **base** (or radix). Each digit's value depends on its position.

| System | Base | Allowed digits | Common use |
|---|---:|---|---|
| Binary | 2 | 0, 1 | Computer logic and representation |
| Octal | 8 | 0 through 7 | Compact grouping of binary digits |
| Decimal | 10 | 0 through 9 | Everyday counting |
| Hexadecimal | 16 | 0 through 9, A through F | Compact representation of binary values |

In base b, a number such as d₃d₂d₁d₀ means:

~~~text
d₃ × b³ + d₂ × b² + d₁ × b¹ + d₀ × b⁰
~~~

### Convert binary to decimal

**Example:** 101101₂

~~~text
1×2⁵ + 0×2⁴ + 1×2³ + 1×2² + 0×2¹ + 1×2⁰
= 32 + 0 + 8 + 4 + 0 + 1
= 45₁₀
~~~

### Convert decimal to binary by repeated division

Divide by 2, record each remainder, and read the remainders from bottom to top.

~~~text
45 ÷ 2 = 22 remainder 1
22 ÷ 2 = 11 remainder 0
11 ÷ 2 =  5 remainder 1
 5 ÷ 2 =  2 remainder 1
 2 ÷ 2 =  1 remainder 0
 1 ÷ 2 =  0 remainder 1
Read upward: 101101₂
~~~

### Binary, octal, and hexadecimal shortcuts

- One octal digit represents exactly **3 binary bits**.
- One hexadecimal digit represents exactly **4 binary bits**.
- Group bits from the right for the integer part. Pad the left with zeros if needed.

~~~text
Binary:       10 1101
Octal groups: 010 110 101?   Do not insert a bit; regroup the original digits.
Correct for 101101₂: 101 101 = 55₈
Hex groups:   0010 1101 = 2D₁₆
~~~

**Cross-check:** 45₁₀ = 101101₂ = 55₈ = 2D₁₆.

### Convert a fraction from binary to decimal

Digits to the right of the binary point have weights 2⁻¹, 2⁻², 2⁻³, and so on.

~~~text
1101.101₂
= 1×8 + 1×4 + 0×2 + 1×1 + 1×1/2 + 0×1/4 + 1×1/8
= 13 + 0.5 + 0.125
= 13.625₁₀
~~~

### Convert a decimal fraction to binary

Multiply the fractional part by 2. Record the integer part each time; read recorded bits from top to bottom.

~~~text
0.625 × 2 = 1.25   record 1
0.25  × 2 = 0.50   record 0
0.50  × 2 = 1.00   record 1
So 0.625₁₀ = 0.101₂
~~~

Some decimal fractions repeat forever in binary, just as 1/3 repeats in decimal. Computers may store a rounded approximation.

---

## 8. Binary arithmetic and signed numbers

### Binary addition rules

~~~text
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 10   (write 0, carry 1)
1 + 1 + 1 = 11   (write 1, carry 1)
~~~

**Worked example: add 1011₂ and 0110₂**

~~~text
   1011
 + 0110
 ------
  10001
~~~

Check in decimal: 11 + 6 = 17, and 10001₂ = 17₁₀.

Subtraction can be done by borrowing, or by adding a number's two's complement in fixed-width signed arithmetic.

### Two's complement

Most general-purpose computers represent signed integers using **two's complement**.

To form the negative of a positive binary value in a fixed number of bits:

1. Invert every bit.
2. Add 1.
3. Keep the same bit width.

**Example: represent −13 in 8 bits**

~~~text
+13 = 00001101
Invert = 11110010
Add 1  = 11110011
Therefore −13 = 11110011 in 8-bit two's complement.
~~~

In n-bit two's complement, the representable range is −2ⁿ⁻¹ through 2ⁿ⁻¹ − 1. For 8 bits, that is −128 through +127. The same bit pattern means different values depending on whether it is interpreted as unsigned or signed.

---

## 9. Character codes and digital media

A computer stores patterns of bits. A **character encoding** defines how bit patterns represent characters.

- **ASCII:** the original ASCII character set uses 7 bits, giving 128 codes. It includes English letters, digits, punctuation, and control characters.
- **Extended ASCII:** a historical informal term for several incompatible 8-bit code pages. It is not one universal standard.
- **Unicode:** a universal character standard that assigns code points to characters from many writing systems and includes symbols and emoji.
- **UTF-8:** a variable-length encoding for Unicode. It uses one to four bytes per code point and preserves the original ASCII byte values for ASCII characters.
- **UTF-16:** another Unicode encoding, using one or two 16-bit code units per code point.
- **EBCDIC:** an 8-bit character encoding historically used on IBM systems.
- **BCD (Binary-Coded Decimal):** stores each decimal digit separately as a 4-bit pattern. For example, decimal 59 is BCD 0101 1001; this differs from binary 111011.

### Digital media basics

- **Image:** often represented as pixels. Each pixel stores colour information; resolution counts pixels across width and height. More pixels can mean more detail, but display size, compression, and image quality also matter.
- **Audio:** a digital recording samples sound many times per second and quantises each sample. Sample rate is measured in samples per second; bit depth describes the precision of each sample.
- **Video:** a sequence of frames plus timing and often audio. Resolution, frame rate, colour depth, and compression affect size and quality.
- **Compression:** lossless compression reconstructs the original data exactly; lossy compression discards some information to reduce size.

---

## 10. Hardware, software, and language translation

- **Hardware:** physical components such as CPU, memory, keyboard, display, and network card.
- **Software:** programs and related data that instruct hardware.
- **System software:** manages the computer and provides a platform, including operating systems, drivers, and utilities.
- **Application software:** performs tasks for users, such as a browser, editor, or spreadsheet.
- **Firmware:** software stored in non-volatile memory that controls a device at a low level.
- **Utility software:** helps maintain, protect, or manage a computer, such as backup or antivirus tools.

### Language translators

- **Assembler:** translates assembly language into machine code.
- **Compiler:** translates a program, often an entire source unit, into another form before execution. A compiled program may be distributed as machine code or an intermediate representation.
- **Interpreter:** executes program instructions through a runtime, often while translating or evaluating them.
- **Linker:** combines compiled object files and libraries and resolves references among them.
- **Loader:** places a program into memory and prepares it to run.
- **Just-in-time (JIT) compiler:** compiles some code during program execution. Real language implementations often mix compilation and interpretation.

~~~mermaid
flowchart LR
    SRC[Source code] --> COMP[Compiler or interpreter]
    COMP --> OBJ[Object code or intermediate form]
    OBJ --> LINK[Linker with libraries]
    LINK --> EXE[Executable or runnable program]
    EXE --> LOAD[Loader]
    LOAD --> RAM[Program in memory]
    RAM --> CPU[CPU executes instructions]
~~~

**Interview caution:** Avoid saying that every language is only compiled or only interpreted. Python, Java, JavaScript, and other languages can use multiple translation stages depending on the implementation.

---

## 11. Operating systems, files, and booting

### Operating system

An **operating system (OS)** is system software that manages hardware resources and provides services for applications and users.

Main responsibilities include:

- **Process management:** starts, schedules, and stops running programs.
- **Memory management:** allocates memory and protects one process from another.
- **File management:** organises files and directories and controls access.
- **Device management:** communicates with devices through drivers.
- **Security and accounts:** authenticates users and enforces permissions.
- **User interface:** provides a graphical user interface (GUI), command-line interface (CLI), or both.
- **Networking:** supports communication over networks.

Examples include Windows, macOS, Linux distributions, Android, and iOS.

### File concepts

- A **file** is a named collection of data.
- A **directory/folder** organises files and other directories.
- A **file extension** is a suffix such as .txt or .pdf that commonly indicates a format. It does not prove the actual contents.
- An **absolute path** identifies a location from the filesystem root. A **relative path** is interpreted from the current location.
- A **filesystem** defines how files and directories are stored and found on a device.
- **Permissions** specify which users or programs can read, write, or execute an item.

### Boot process

**Booting** is the process of starting a computer and loading its operating system.

~~~mermaid
flowchart LR
    P[Power on] --> F[Firmware: UEFI or BIOS]
    F --> H[Hardware checks and device setup]
    H --> B[Find boot device]
    B --> L[Bootloader]
    L --> K[Load operating-system kernel]
    K --> S[Start system services]
    S --> U[Login or user interface]
~~~

Modern systems commonly use UEFI firmware; BIOS remains common as a term and in older systems. Exact startup details vary by device and configuration.

---

## 12. Computer networks, Internet, and web

A **computer network** connects devices so they can exchange data and share resources.

| Network scope | Typical coverage |
|---|---|
| PAN | Personal area, such as devices around one person |
| LAN | Local area, such as a room, home, office, or school |
| MAN | Metropolitan area, across a city or large campus |
| WAN | Wide area, across large geographic regions |

- **Switch:** connects devices within a local network and forwards frames using local hardware addresses.
- **Router:** forwards packets between networks using IP addresses. A home router often combines a router, switch, wireless access point, and firewall.
- **Modem/ONT:** connects a customer network to an internet service provider's access technology.
- **Access point:** provides wireless devices a connection to a wired network.
- **Protocol:** agreed rules and formats for communication.
- **Bandwidth:** a link's data-carrying capacity, usually stated in bits per second. Actual throughput may be lower.

### Internet and World Wide Web

The **Internet** is a global network of interconnected networks that use the Internet Protocol suite. The **World Wide Web** is a system of linked resources accessed over the Internet using web technologies such as HTTP. Email, voice calls, and many other services use the Internet but are not themselves the Web.

### What happens when a website opens?

~~~mermaid
sequenceDiagram
    participant U as User browser
    participant D as DNS resolver
    participant W as Web server
    U->>D: Ask for the IP address of a domain
    D-->>U: Return an address
    U->>W: Connect and request a page using HTTPS
    W-->>U: Return response and page resources
    U->>U: Render HTML, CSS, and JavaScript
~~~

- **IP address:** identifies a network interface or destination for packet delivery.
- **DNS (Domain Name System):** looks up names such as example.com and returns records, often including IP addresses.
- **URL:** address of a resource. Example: in https://example.com/notes?unit=1, the scheme is https, the host is example.com, the path is /notes, and the query is unit=1.
- **HTTP:** application protocol for web requests and responses.
- **HTTPS:** HTTP protected by TLS. TLS encrypts the connection and helps authenticate the server. HTTPS does not prove that a website is honest or that its content is safe.
- **HTML:** describes page structure and content.
- **CSS:** describes presentation and layout.
- **JavaScript:** commonly adds program logic and interactions in a browser.

### Email and cloud

- **SMTP:** commonly used to send or relay email.
- **IMAP:** commonly synchronises and manages messages stored on a mail server.
- **POP3:** commonly downloads messages from a mail server, with behaviour depending on settings.
- **Cloud computing:** provides shared computing resources over a network. Users may consume software, platforms, or infrastructure as services. Cloud data still depends on providers, networks, access control, backups, and good security practices.

---

## 13. Cybersecurity and safe digital practice

The **CIA triad** is a basic security model:

- **Confidentiality:** information is seen only by authorised people.
- **Integrity:** information remains correct and is changed only in authorised ways.
- **Availability:** systems and information are accessible when needed.

### Common threats

- **Malware:** harmful software, including viruses, worms, trojans, spyware, and ransomware.
- **Phishing:** deceptive messages or pages that try to steal credentials or persuade a user to act.
- **Weak or reused passwords:** make account compromise more likely.
- **Social engineering:** manipulation of people to reveal information or grant access.
- **Data loss:** can result from device failure, accidental deletion, theft, or ransomware.
- **Unpatched software:** may retain known security weaknesses.

### Practical protections

- Use long, unique passwords and a password manager.
- Turn on multi-factor authentication where available.
- Install updates from trusted sources.
- Check links, sender identity, and unexpected requests before acting.
- Keep more than one backup, including a copy separated from the computer.
- Limit permissions and lock devices when unattended.
- Use secure networks and HTTPS, while remembering that HTTPS alone cannot establish trust in a website.
- Protect personal data and get consent before sharing other people's information.

---

## 14. ICT in education and public services

ICT supports teaching, learning, administration, and public access to information.

- **Teaching:** presentations, simulations, digital content, educational software, and accessible learning resources.
- **Communication:** announcements, email, video meetings, and learning platforms.
- **Assessment:** quizzes, digital submissions, data analysis, and feedback.
- **Administration:** student records, scheduling, attendance, and reports.
- **E-governance:** online delivery of public services and information.
- **Digital inclusion:** access should account for devices, connectivity, language, disability, affordability, and digital skills.
- **Digital citizenship:** responsible, respectful, safe participation online; includes privacy, copyright, evaluating sources, and avoiding cyberbullying.

**Teacher's example:** A digital quiz can return answers quickly, but a teacher still needs to check that the questions are fair, accessible, aligned with learning goals, and interpreted correctly.

---

## 15. Solved revision problems

### Problem 1: Convert 156₁₀ to hexadecimal

~~~text
156 ÷ 16 = 9 remainder 12
Remainder 12 is C in hexadecimal.
Therefore 156₁₀ = 9C₁₆.
Check: 9×16 + 12 = 156.
~~~

### Problem 2: Convert 3A₁₆ to decimal

~~~text
3×16¹ + A×16⁰
= 3×16 + 10×1
= 58₁₀
~~~

### Problem 3: Convert 111010₂ to octal

~~~text
Group from right: 111 010
111₂ = 7 and 010₂ = 2
Therefore 111010₂ = 72₈.
~~~

### Problem 4: Convert 2.5 MB to bits

Use decimal MB: 2.5 MB = 2,500,000 bytes.

~~~text
2,500,000 × 8 = 20,000,000 bits = 20 Mb
~~~

This is a unit conversion, not a claim about transfer time; real network throughput also has overhead.

### Problem 5: Explain why the operating system uses virtual memory

Virtual memory gives each process a managed address space and can move some less-active pages between RAM and storage. It can help when RAM is insufficient, but storage is much slower than RAM, so heavy paging often slows the computer.

---

## 16. Short-answer questions with answers

1. **What is a computer?**  
   An electronic programmable system that accepts data and instructions, processes them, produces output, and stores data or results.

2. **How are data and information different?**  
   Data are raw facts; information is data organised or processed to be useful.

3. **What does the ALU do?**  
   It performs arithmetic and logical operations.

4. **What does the control unit do?**  
   It fetches and decodes instructions and coordinates the CPU and other components with control signals.

5. **What is a register?**  
   A small, fast storage location inside the CPU.

6. **Why is RAM called volatile?**  
   Its contents normally disappear when power is removed.

7. **How does an SSD differ from an HDD?**  
   An SSD uses flash memory and has no moving disk platter; an HDD uses magnetic rotating platters and moving parts.

8. **What is cache memory?**  
   Fast, small memory that keeps copies of data or instructions likely to be needed soon.

9. **How many bits are in a byte?**  
   Eight.

10. **What is the base of hexadecimal?**  
    Sixteen.

11. **What is the binary equivalent of 45₁₀?**  
    101101₂.

12. **What is Unicode?**  
    A character standard that assigns code points to characters from many writing systems and symbol sets.

13. **What is the main role of an operating system?**  
    To manage hardware and provide common services and interfaces for applications and users.

14. **How do the Internet and the Web differ?**  
    The Internet is the interconnected network infrastructure; the Web is a linked-resource service that uses that infrastructure.

15. **What does DNS do?**  
    It looks up domain names and returns records such as IP addresses.

16. **What is the purpose of HTTPS?**  
    It protects HTTP communication with TLS, including encryption and server authentication.

17. **Name the CIA triad.**  
    Confidentiality, integrity, and availability.

18. **Why is a backup useful?**  
    It provides a separate copy from which data may be restored after loss or damage.

19. **What does OCR do?**  
    It recognises text in an image or scanned page and produces machine-readable text.

20. **What is the fetch–decode–execute cycle?**  
    The CPU obtains an instruction, determines its meaning, performs it, and stores a result if needed.

---

## 17. Multiple-choice practice

Choose one answer, then check the key.

1. Which component performs arithmetic and logical operations?  
   A. ALU  B. Printer  C. SSD  D. Scanner

2. Which memory is usually volatile?  
   A. RAM  B. DVD  C. Flash drive  D. HDD

3. Which is the correct binary representation of decimal 10?  
   A. 1001  B. 1010  C. 1100  D. 1110

4. The base of octal is:  
   A. 2  B. 8  C. 10  D. 16

5. One byte contains:  
   A. 4 bits  B. 7 bits  C. 8 bits  D. 16 bits

6. Which device converts a printed image to digital pixels?  
   A. Scanner  B. Speaker  C. Projector  D. Plotter

7. Which device forwards packets between networks?  
   A. Router  B. Keyboard  C. Monitor  D. Plotter

8. What does DNS commonly look up for a domain?  
   A. An IP address  B. A keyboard layout  C. A file extension  D. A CPU instruction

9. Which protocol is commonly used to send email?  
   A. SMTP  B. OCR  C. ALU  D. HTML

10. Which is system software?  
    A. Operating system  B. Photo album  C. Presentation file  D. Digital photograph

11. Which is a non-volatile storage device?  
    A. SSD  B. CPU register  C. Cache  D. DRAM

12. Which encoding is variable length and can represent Unicode characters?  
    A. UTF-8  B. BCD only  C. ASCII 7-bit only  D. Unsigned integer

13. The CPU's control unit primarily:  
    A. Coordinates instruction execution  B. Prints documents  C. Stores every file permanently  D. Captures sound

14. HTTPS protects web communication using:  
    A. TLS  B. OCR  C. SMTP  D. BCD

15. A LAN is usually a network covering:  
    A. A limited local area  B. The whole planet only  C. One CPU register  D. A single file

16. In a positional number, digit value depends on:  
    A. Its position and the base  B. Its font  C. Its file extension  D. The monitor size

17. Which statement about a compiler is most accurate?  
    A. It translates source code to another form before execution  
    B. It is always the operating system  
    C. It is an output device  
    D. It stores files permanently

18. What is the first stage in the basic fetch–decode–execute cycle?  
    A. Fetch  B. Execute  C. Print  D. Back up

19. Which part of the CIA triad means information is changed only in authorised ways?  
    A. Integrity  B. Availability  C. Confidentiality  D. Compression

20. Which is a separate recovery copy of data?  
    A. Backup  B. Cache  C. Register  D. Browser tab

### Answer key with brief reasons

1. A — the ALU performs arithmetic and logic.  
2. A — RAM is normally volatile.  
3. B — 8 + 2 = 10.  
4. B — octal has eight digits, 0 through 7.  
5. C — one byte is eight bits.  
6. A — scanner captures the page image.  
7. A — router forwards packets between networks.  
8. A — DNS can return an IP address record.  
9. A — SMTP is commonly used for sending or relaying email.  
10. A — an OS manages the computer and provides services to applications.  
11. A — SSD flash retains data without power.  
12. A — UTF-8 is a variable-length Unicode encoding.  
13. A — the control unit directs and coordinates operations.  
14. A — HTTPS uses TLS to protect HTTP communication.  
15. A — a LAN covers a local area.  
16. A — positional value is determined by digit, position, and base.  
17. A — compilation translates source to a target form before execution.  
18. A — the cycle begins by fetching an instruction.  
19. A — integrity concerns authorised and correct changes.  
20. A — a backup is a recovery copy.

---

## 18. Long-answer, teaching, and interview practice

Try each answer without looking at the notes. In an exam, define the term, draw a labelled diagram where relevant, give one example, and end with a comparison or use.

1. Draw the functional block diagram of a computer and explain each unit.
2. Explain the fetch–decode–execute cycle with a labelled flowchart.
3. Compare RAM, ROM/firmware, cache, and secondary storage by speed, volatility, and use.
4. Convert 93₁₀ to binary, octal, and hexadecimal. Show every step.
5. Explain binary addition and represent −18 using 8-bit two's complement.
6. Explain how a browser finds and displays a website. Include DNS, IP, HTTPS, server, HTML, CSS, and JavaScript.
7. Explain the difference between the Internet and the World Wide Web.
8. Explain the role of ICT in a classroom. Include inclusion, accessibility, assessment, privacy, and teacher oversight.
9. **Interview prompt:** A laptop is slow when many apps are open, but it has plenty of free disk space. Explain two possible causes and how you would investigate.
10. **Interview prompt:** Why can a program's output be wrong even when the computer is functioning correctly? Discuss input, requirements, algorithm, implementation, and testing.

### Model outline for question 9

- Free disk space and available RAM are different resources.
- Many active applications may use more RAM than available.
- The OS may page memory to storage; storage is slower than RAM.
- Other possible causes include CPU load, background tasks, thermal limits, or a slow drive.
- Inspect resource usage, reproduce the issue, identify the largest process, and change one factor at a time.
- Do not conclude that adding RAM is necessary until evidence supports it.

### Model outline for question 10

- The computer follows the instructions it receives.
- Input can be incomplete, inaccurate, or in the wrong format.
- Requirements may be ambiguous or misunderstood.
- The algorithm may not cover an edge case.
- The implementation may contain a defect.
- Testing may omit realistic cases.
- Verify expected behaviour with examples and boundary cases, then trace the stages where the result changes.

---

## 19. Flashcards

| Front | Back |
|---|---|
| What does IPO stand for? | Input, processing, output; storage supports the cycle. |
| What does the ALU do? | Arithmetic and logical operations. |
| What does the control unit do? | Coordinates instruction execution using control signals. |
| How many bits are in a byte? | 8 bits. |
| Which is volatile: RAM or SSD? | RAM is usually volatile; SSD is non-volatile. |
| How many binary bits form one hexadecimal digit? | 4 bits. |
| What is 45 in binary? | 101101₂. |
| What does OCR do? | Recognises text in an image or scan. |
| What does an operating system manage? | Processes, memory, files, devices, security, and user services. |
| What does DNS do? | Looks up domain names and returns DNS records. |
| How are Internet and Web different? | The Internet is the network; the Web is one service running over it. |
| What does HTTPS add to HTTP? | TLS protection for the connection. |
| Name the CIA triad. | Confidentiality, integrity, availability. |
| Which device routes packets between networks? | Router. |
| Why is cache used? | To keep frequently used data close to the CPU. |

---

## 20. One-page quick revision

~~~text
COMPUTER = input + processing + output + storage
CPU = ALU + control unit + registers
CPU cycle = fetch -> decode -> execute -> store
Memory speed: registers -> cache -> RAM -> SSD/HDD (generally slower)
RAM: volatile working memory
SSD/HDD: persistent storage
1 byte = 8 bits; 1 nibble = 4 bits
Binary base 2 | Octal base 8 | Decimal base 10 | Hex base 16
45 decimal = 101101 binary = 55 octal = 2D hexadecimal
OS manages processes, memory, files, devices, security, and services
Internet = network of networks; Web = linked resources using web protocols
DNS looks up names; router forwards packets; HTTPS protects HTTP with TLS
CIA = confidentiality, integrity, availability
Backup = separate recovery copy
~~~

### Self-check before moving to the next subject

- Can I redraw the IPO and CPU diagrams without looking?
- Can I explain RAM versus storage in one sentence?
- Can I convert a decimal integer to binary and check the answer?
- Can I group binary digits into octal and hexadecimal?
- Can I explain DNS and HTTPS without confusing their roles?
- Can I answer one classroom question and one product-interview question using a clear example?

**Next module in the sequence:** Programming Fundamentals — algorithms, flowcharts, variables, data types, operators, control flow, functions, debugging, and practice questions.
