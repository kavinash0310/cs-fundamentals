# Chapter 1 — Problem Solving, Algorithms, and C Foundations

**Course:** Programming Fundamentals  
**Primary exam language:** C  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Computer fundamentals

## What you will learn

You will turn a statement into a precise algorithm, represent logic using pseudocode and flowcharts, understand how C source becomes an executable, recognise C tokens and types, evaluate expressions carefully, and distinguish defined behavior from undefined behavior.

---

## 1.1 Start from the specification

Before writing code, identify:

- **Input:** values the program receives.
- **Output:** exact results required.
- **Preconditions:** what must be true before execution.
- **Postconditions:** what must be true after successful execution.
- **Constraints:** allowed ranges, sizes, time, and memory.
- **Edge cases:** empty input, zero, negative values, duplicates, maximum values, or invalid data.

**Example — find the largest value in an array**

Input: n and n integers, with n > 0.  
Output: the largest integer.  
Algorithm: start with the first value as current maximum; inspect every remaining value; replace the maximum when a larger value is found.  
Edge cases: n=1, all values negative, repeated maximum, maximum at first/last position.

An assumption such as n>0 must be stated. Otherwise, reading the first element of an empty array is invalid.

---

## 1.2 Algorithm, pseudocode, and flowchart

An **algorithm** is a finite, ordered, unambiguous method for solving a problem. Pseudocode explains the method independently of exact language syntax. A flowchart uses symbols and arrows to show control flow. A program implements the method in a language.

### Pseudocode: sum n numbers

~~~text
READ n
total ← 0
FOR each of n input values
    READ value
    total ← total + value
OUTPUT total
~~~

### Flowchart: positive, negative, or zero

~~~mermaid
flowchart TD
    A([Start]) --> B[/Read x/]
    B --> C{x > 0?}
    C -- Yes --> P[/Print positive/]
    C -- No --> D{x < 0?}
    D -- Yes --> N[/Print negative/]
    D -- No --> Z[/Print zero/]
    P --> E([End])
    N --> E
    Z --> E
~~~

### Properties of a good algorithm

- It terminates.
- Each step has a clear meaning.
- It gives correct results for all allowed inputs.
- It handles specified boundary cases.
- Its resource use is reasonable for expected input sizes.

---

## 1.3 Basic algorithm analysis

Let n denote input size.

- **Time complexity** describes how the number of operations grows with n.
- **Space complexity** describes how additional memory grows with n.
- **Worst-case complexity** bounds the most work for any input of size n.
- **Average-case complexity** depends on an assumed distribution of inputs.
- **Best-case complexity** describes the least work.

Common growth classes:

| Class | Typical example |
|---|---|
| O(1) | Read array element at a known index |
| O(log n) | Halve a search range repeatedly |
| O(n) | Scan every input value |
| O(n log n) | Efficient comparison sorting |
| O(n²) | Compare each pair of values |

Big O describes asymptotic growth, not exact seconds. Constants and hardware matter for small inputs.

### Worked example: count comparisons in a nested loop

~~~c
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        count++;
    }
}
~~~

The inner statement runs n times for each of n outer iterations, so it runs n² times. Time complexity is O(n²); extra space is O(1).

---

## 1.4 From C source to execution

C is a compiled, statically typed language. A typical toolchain performs stages like these:

~~~mermaid
flowchart LR
    C[.c source files] --> PP[Preprocessor]
    PP --> CC[Compiler]
    CC --> ASM[Assembler]
    ASM --> OBJ[Object files]
    OBJ --> LINK[Linker and libraries]
    LINK --> EXE[Executable]
    EXE --> LOAD[Loader and runtime]
~~~

- **Preprocessing:** processes directives such as #include and #define.
- **Compilation:** checks and translates C source to assembly or an intermediate representation.
- **Assembly:** produces machine-code object files.
- **Linking:** resolves references between object files and libraries.
- **Loading:** places executable segments in memory and starts the program.

A **compiler error** may occur before a program runs. A **linker error** often means a function or object was declared but no matching definition was found. A **runtime error** happens during execution. A **logic error** produces the wrong answer despite running.

### Minimal complete C program

~~~c
#include <stdio.h>

int main(void) {
    printf("Hello, computer science!\n");
    return 0;
}
~~~

- #include <stdio.h> makes standard input/output declarations available.
- int main(void) is the program entry point in a hosted C environment.
- Braces delimit a block.
- A semicolon ends most declarations and statements.
- return 0 indicates successful completion by convention.

---

## 1.5 Tokens and names

A **token** is a basic unit recognized by the compiler.

- **Keywords:** reserved words such as int, return, if, and while.
- **Identifiers:** names created by the programmer.
- **Constants/literals:** values written directly, such as 42, 3.5, 'A', and "hello".
- **Operators:** symbols such as +, *, ==, and &&.
- **Punctuators:** symbols such as semicolon, comma, parentheses, and braces.

Identifier rules include:

- Usually begin with a letter or underscore; cannot begin with a digit.
- May contain letters, digits, and underscore.
- Are case-sensitive.
- Must not be a reserved keyword.
- Should describe purpose: student_count is clearer than x1.

Comments are for people and are ignored as ordinary program behavior. C supports /* block comments */ and // line comments in modern C.

---

## 1.6 Variables, declarations, and types

A **declaration** gives a name and type information to the compiler.

~~~c
int student_count = 35;
double temperature = 29.5;
char grade = 'B';
~~~

A type determines the set of representable values, storage rules, and permitted operations.

### Fundamental types

- **Integer types:** char, short, int, long, long long, each possibly signed or unsigned where allowed.
- **Floating types:** float, double, long double.
- **Boolean:** C99 provides _Bool; including stdbool.h provides bool, true, and false macros.
- **void:** no value or unspecified object type in certain contexts.

Do not memorise that int is always exactly 32 bits. C specifies minimum ranges and implementation-dependent sizes. Use sizeof and limits from limits.h or stdint.h when code depends on exact widths.

### Integer width and ranges

For an unsigned N-bit integer, the range is 0 to 2^N−1. For common two's-complement signed N-bit integers, the range is −2^(N−1) to 2^(N−1)−1. Use a fixed-width type such as uint32_t only when the implementation provides it in stdint.h.

### Object, value, and address

- An **object** is a region of data storage with a type.
- A **value** is the information currently represented by that object.
- An **address** identifies where the object is stored in an execution environment.
- A **variable name** designates an object in source code.

Optimising compilers may keep values in registers or eliminate storage entirely; source-level names do not guarantee a fixed physical address.

---

## 1.7 Constants and type qualifiers

~~~c
const double PI_APPROX = 3.141592653589793;
~~~

const means the program cannot modify the object through that qualified access path. It does not always mean the object is a compile-time constant expression in C.

~~~c
int value = 10;
const int *p1 = &value;   /* cannot modify value through p1 */
int *const p2 = &value;   /* p2 cannot be changed to point elsewhere */
const int *const p3 = &value; /* neither through p3 */
~~~

volatile tells the compiler that an object may change in ways not visible in ordinary code, such as a memory-mapped device register. It is not a thread-synchronization mechanism.

---

## 1.8 Expressions, assignment, and comparison

An **expression** computes a value. A **statement** performs an action or controls execution.

~~~c
int total = 4 + 6 * 2;
int passed = (total >= 15);
~~~

Multiplication has higher precedence than addition, so total is 16. Parentheses make intent clear.

- **=** assigns a value.
- **==** compares for equality.
- **!=** compares for inequality.
- **<, <=, >, >=** compare values.
- In C, a comparison produces integer 0 or 1.
- Logical AND is **&&**, logical OR is **||**, logical NOT is **!**.

A common bug is writing **if (x = 5)** instead of **if (x == 5)**. Assignment expression has a value, so this code may compile and behave unexpectedly. Enable compiler warnings and read them.

### Arithmetic operators

Addition, subtraction, multiplication, division, and remainder are written +, -, *, /, and %.

For integer operands, division discards the fractional part by truncating toward zero in modern C:

~~~text
 7 / 3 =  2
-7 / 3 = -2
-7 % 3 = -1
~~~

The quotient q and remainder r satisfy a = q×b + r.

### Short-circuit logic

For A && B, B is evaluated only when A is nonzero. For A || B, B is evaluated only when A is zero.

~~~c
if (denominator != 0 && numerator / denominator > 2) {
    puts("Large quotient");
}
~~~

The first condition prevents division by zero because the second operand is skipped when denominator is zero.

---

## 1.9 Conversions and arithmetic

C may convert values implicitly. A conversion can preserve, round, truncate, or lose information.

~~~c
int whole = (int) 3.9;  /* whole is 3 */
double average = 5 / 2;  /* average is 2.0: integer division happened first */
double correct = 5.0 / 2; /* 2.5 */
~~~

To get a fractional result, ensure at least one operand is floating point before division.

### Integer promotions

Small integer types such as char and short are commonly promoted to int or unsigned int before arithmetic. Integer constants also have types chosen by rules depending on their form. The resulting type of a mixed expression depends on the integer conversion rules.

For exams, evaluate in this order:

1. Determine each operand's type.
2. Apply integer promotions.
3. Apply the usual arithmetic conversions.
4. Perform the operation in the resulting type.
5. Convert to the assignment or return type.

Do not assume assignment to a wider type makes an earlier narrow calculation safe.

---

## 1.10 Signed and unsigned arithmetic

Unsigned arithmetic wraps modulo 2^N for an N-bit unsigned type.

~~~c
unsigned int x = 0;
x = x - 1;  /* wraps to the largest value of unsigned int */
~~~

Signed overflow is **undefined behavior** in C. The language does not promise wraparound.

~~~c
#include <limits.h>

int x = INT_MAX;
x = x + 1; /* undefined behavior if the mathematical result is not representable */
~~~

A compiler may optimize under the assumption that undefined behavior does not occur. Do not use overflow as a shortcut.

Mixing signed and unsigned operands can convert a negative signed value to a large unsigned value, yielding surprising comparisons. Use matching types or explicitly validate and convert values.

---

## 1.11 Floating point and equality

Floating-point values are often approximations. Avoid comparing calculated real values for exact equality unless the values are guaranteed exact by the operation.

~~~c
double x = 0.1 + 0.2;
if (x == 0.3) {
    /* may not behave as intended */
}
~~~

For a tolerance-based comparison:

~~~c
#include <math.h>

if (fabs(x - expected) < 1e-9) {
    /* close enough for the chosen scale and tolerance */
}
~~~

The correct tolerance depends on the scale and numerical problem. Floating-point accuracy is covered further in the data-representation chapter.

---

## 1.12 C standard and portability

C programs should state or target a language standard supported by the compiler. Common editions include C99, C11, C17, and C23. Some details, available library features, and diagnostics vary by standard and implementation.

- **Portable code** relies on behavior specified by the standard or explicitly checks implementation properties.
- **Implementation-defined behavior** is chosen by the implementation and documented.
- **Unspecified behavior** permits one of several choices without requiring documentation of which choice is made.
- **Undefined behavior (UB)** means the C standard imposes no requirements; the program is not portable and the compiler may assume the behavior never happens.

Examples of UB include signed integer overflow, out-of-bounds array access, dereferencing an invalid pointer, and modifying a string literal through a non-const pointer.

---

## 1.13 Worked examples

**1. Evaluate a mixed expression**

~~~c
int a = 7;
int b = 2;
double x = a / b;
double y = (double)a / b;
~~~

a / b uses integer division, so x becomes 3.0. In y, a is converted to double before division, so y becomes 3.5.

**2. Find the type-effect bug**

~~~c
int sum = 0;
double average = sum / 3;
~~~

This produces 0.0 because integer division happens first. Use sum / 3.0 or explicitly convert sum to double.

**3. Identify a likely assignment bug**

~~~c
if (count = 0) {
    puts("empty");
}
~~~

This assigns zero to count, and the resulting condition is false. If the intent is comparison, write count == 0. A compiler warning should flag suspicious assignment in a condition.

---

## 1.14 Questions and answers

1. **What is the difference between an algorithm and a program?**  
   An algorithm is a language-independent method; a program implements it in a language.

2. **What does a compiler do?**  
   It translates source code to another representation and diagnoses many source-level errors.

3. **What does a linker do?**  
   It combines object files/libraries and resolves external references.

4. **What does sizeof report?**  
   The size in bytes of a type or object as determined by the implementation.

5. **Is int always 32 bits?**  
   No. Its size and range depend on the implementation, subject to standard minimum requirements.

6. **What is the difference between = and ==?**  
   Assignment versus equality comparison.

7. **What is the result of 7/2 when both operands are int?**  
   3.

8. **How can short-circuit evaluation prevent division by zero?**  
   Put the nonzero check first in an && expression.

9. **Does converting an overflowing signed result to a larger variable repair it?**  
   No. The overflow occurred during the earlier expression.

10. **What does undefined behavior mean?**  
    The C standard places no requirements on what happens; the result must not be relied on.

### GATE/BPSC practice with solutions

1. What is printed?

~~~c
int a = 5;
double b = a / 2;
printf("%.1f\n", b);
~~~

**Answer:** 2.0. Both operands of division are int, so integer division produces 2 before conversion to double.

2. What is printed?

~~~c
int a = 5;
double b = (double)a / 2;
printf("%.1f\n", b);
~~~

**Answer:** 2.5. The cast makes division floating point.

3. Does unsigned 8-bit value 0 minus 1 wrap or overflow undefinedly?  
**Answer:** Wraps modulo 256 to 255, assuming an 8-bit unsigned type.

4. Why can **if (x = 1)** be legal C?  
**Answer:** Assignment is an expression whose result is the assigned value; nonzero is true. It may be unintended and should be corrected or explicitly parenthesized when intentional.

5. State one example each of implementation-defined behavior and undefined behavior.  
**Answer:** The size of int is implementation-defined. Signed overflow is undefined behavior.

### Multiple-choice check

1. C program entry point in a hosted environment is normally: A. start B. main C. printf D. include  
2. Integer division 9/4 gives: A. 2 B. 2.25 C. 3 D. 0  
3. Which operator compares equality? A. = B. == C. != D. &&  
4. What is required for safe division in a/b? A. check b != 0 B. use unsigned b C. use printf D. use char  
5. Signed overflow in C is: A. always wraparound B. undefined behavior C. a syntax error D. guaranteed saturation  
6. sizeof is measured in: A. bits B. bytes C. words only D. seconds

**Answers:** 1-B, 2-A, 3-B, 4-A, 5-B, 6-B.

### Draw-from-memory task

Draw the C translation pipeline from source through preprocessing, compilation, assembly, linking, loading, and execution. Beside it, place one error that can occur at each stage.
