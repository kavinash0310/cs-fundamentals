# Programming Fundamentals — Course Index

This is a chapter-based course in **C**, the language named in the GATE CS programming syllabus and a useful foundation for many computer-science teacher exams. It builds from algorithms and expressions to arrays, pointers, memory, files, and recursion.

Python can be used for school teaching or interviews, but GATE questions in this area require C-language reasoning. The concepts transfer; syntax and memory rules do not.

## How to study

1. Write each code example by hand and predict its result before reading the explanation.
2. Make a trace table for every loop, pointer, and recursive call.
3. When tracing C, write each object's type and value; check conversions and sequencing.
4. If behavior is undefined, state that no portable output is determined instead of guessing.
5. Solve the workbook again without looking at the answer key.
6. Keep an error log for off-by-one, integer conversion, pointer, string, and boundary-case mistakes.

## Chapter sequence

| Chapter | Coverage |
|---|---|
| [1. Problem solving, algorithms, and C foundations](programming-fundamentals/chapter-01-algorithms-and-c-foundations.md) | Specifications, pseudocode, flowcharts, complexity primer, C translation, tokens, types, conversions, overflow and portability |
| [2. Expressions, control flow, and tracing](programming-fundamentals/chapter-02-expressions-control-flow-and-tracing.md) | C conditions, if/switch, loops, invariants, precedence, sequencing, short-circuiting, trace tables |
| [3. Functions, arrays, and strings](programming-fundamentals/chapter-03-functions-arrays-and-strings.md) | Prototypes, pass-by-value, scope, storage duration, array decay, multidimensional arrays, row-major layout, safe strings |
| [4. Pointers, memory, and dynamic allocation](programming-fundamentals/chapter-04-pointers-and-memory.md) | Pointer declarations/arithmetic, arrays versus pointers, const, pointer-to-array, allocation, ownership, memory errors |
| [5. Structures, unions, preprocessing, and files](programming-fundamentals/chapter-05-structures-files-and-preprocessor.md) | Structs and padding, unions, enums, macros, header guards, streams, file errors, command-line arguments |
| [6. Recursion, call stack, and recursive tracing](programming-fundamentals/chapter-06-recursion-and-call-stack.md) | Base cases, stack frames, factorial, GCD, power, Fibonacci, recursive binary search, termination proofs |
| [7. C programming workbook and exam practice](programming-fundamentals/chapter-07-c-programming-workbook.md) | Fully worked traces, short answers, 25 original MCQs with explanations, coding practice and exam traps |

## Exam coverage and boundaries

### GATE CS

The official GATE CS 2027 section called Programming and Data Structures explicitly includes programming in C and recursion, followed by arrays, stacks, queues, linked lists, trees, binary search trees, binary heaps, and graphs.

This course teaches the C language and recursion. It covers array syntax and memory as C concepts. **The data structures themselves and their algorithms are covered separately** in [Data Structures and Algorithms](data-structures-algorithms.md). Complete GATE preparation also requires every other official section, including Engineering Mathematics, Digital Logic, COA, Algorithms, Theory of Computation, Compiler Design, OS, Databases, and Networks.

### Bihar computer-teacher exams

C, algorithms, arrays, functions, and tracing support computer-science subject preparation. Some school curricula and recruitment exams use Python or another language, so compare this C course with the current notification and the [School Computer Science](school-computer-science.md) course. Teacher preparation also includes pedagogy and any general or language papers named by the notification.

## Further courses

- [Data Structures and Algorithms](data-structures-algorithms.md)
- [Object-oriented programming and design](oop-and-design.md)
- [School computer science](school-computer-science.md)
- [Product company interview track](../tracks/product-interviews.md)
- [Bihar computer-teacher track](../tracks/bihar-computer-teacher.md)

## Official starting points

- [GATE 2027 CS syllabus — IIT Madras](https://gate2027.iitm.ac.in/static/doc/GATE2027_Syllabus/CS_GATE2027_Syllabus.pdf)
- [BPSC syllabus page](https://bpsc.bihar.gov.in/syllabus/)
- [BPSC notices](https://bpsc.bihar.gov.in/page/44/)
- [BPSC 2023 Higher Secondary teacher question booklet](https://bpsc.bihar.gov.in/wp-content/uploads/2025/01/NB-2023-07-11-09.pdf)

**Syllabus note:** GATE 2027 is the version used for this map, checked on 30 October 2026. Recheck the official syllabus for your exam year. BPSC requirements are recruitment-specific; the latest official notice takes priority.
