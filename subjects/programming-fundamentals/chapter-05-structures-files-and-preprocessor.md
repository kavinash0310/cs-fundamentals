# Chapter 5 — Structures, Unions, Preprocessing, and Files in C

**Course:** Programming Fundamentals  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–4

## What you will learn

You will group data using structures, compare structures with unions, use enumerations and typedef, understand padding and alignment, write safe preprocessor macros, and perform basic text and binary file operations.

---

## 5.1 Structures

A **structure** groups named fields, potentially of different types, into one object.

~~~c
struct Student {
    int roll;
    char grade;
    double average;
};

struct Student s;
s.roll = 17;
s.grade = 'A';
s.average = 84.5;
~~~

Use the dot operator to access a field. If p is a pointer to a structure, use the arrow operator, which is shorthand for dereferencing and member selection:

~~~c
struct Student *p = &s;
p->roll = 18;        /* equivalent to (*p).roll = 18 */
~~~

A structure assignment copies its fields as a value:

~~~c
struct Student t = s;
~~~

If a structure contains pointer fields, a structure copy copies pointer values, not the pointed-to objects. This is a shallow copy.

---

## 5.2 Structures and padding

The compiler may insert **padding** between structure members or at the end to satisfy alignment requirements.

~~~c
struct Example {
    char c;
    int i;
    char d;
};
~~~

Although the member sizes might sum to six bytes on a particular system, sizeof(struct Example) can be larger because of padding. The exact result depends on the implementation, type sizes, and alignment rules.

Why padding exists: aligned accesses can be more efficient or required by the processor. Reordering fields can sometimes reduce size, but do so only when it does not harm clarity, compatibility, or required layout.

Use offsetof from stddef.h to inspect member offsets on a specific implementation. Do not assume an exact portable struct size unless the format is explicitly controlled.

---

## 5.3 Arrays of structures and nested structures

~~~c
struct Point {
    double x;
    double y;
};

struct Rectangle {
    struct Point top_left;
    struct Point bottom_right;
};

struct Point points[3] = {{0, 0}, {1, 2}, {3, 4}};
~~~

Each array element is a complete structure. A structure can contain other structures by value.

### Self-referential structures

A structure may contain a pointer to its own structure type, which is how linked nodes are represented:

~~~c
struct Node {
    int data;
    struct Node *next;
};
~~~

It cannot contain a complete instance of itself by value, because that would require infinite size. The pointer has a fixed size and refers to another node.

---

## 5.4 Unions

A **union** lets several members share the same memory region. At most one member's stored representation is normally treated as the active value at a time.

~~~c
union Number {
    int integer_value;
    float real_value;
};
~~~

The union size is at least as large as its largest member and may include alignment padding. Its members begin at the same address.

~~~mermaid
flowchart LR
    U[Union storage] --> I[integer_value view]
    U --> F[real_value view]
    U --> B[byte view]
~~~

Writing one member and reading a different member is not a general portable conversion method. Use an explicit conversion or memcpy-based representation handling with rules appropriate to the target.

A **tagged union** stores a separate enum or tag indicating which member is active:

~~~c
enum ValueKind { VALUE_INT, VALUE_FLOAT };

struct Value {
    enum ValueKind kind;
    union {
        int i;
        float f;
    } data;
};
~~~

The tag is a convention; every branch must respect it.

---

## 5.5 Enumerations and typedef

An **enum** gives names to integer-like values.

~~~c
enum Day {
    MONDAY = 1,
    TUESDAY,
    WEDNESDAY
};
~~~

If no explicit value is supplied, the next enumerator is one greater than the previous value. Enums make code more descriptive than unexplained numeric constants.

**typedef** creates an alias for a type:

~~~c
typedef struct Student Student;

struct Student {
    int roll;
    double average;
};
~~~

typedef does not create a new distinct type; it creates another name for the same type.

---

## 5.6 Bit-fields

A bit-field specifies a number of bits for an integer-like structure member:

~~~c
struct Flags {
    unsigned int ready : 1;
    unsigned int mode : 3;
};
~~~

Bit-field layout, allocation order, and alignment have implementation-defined details. Do not use bit-fields for a portable file or network format unless the implementation is controlled and documented. Bit masks on fixed-width unsigned integers are often clearer for hardware protocols.

---

## 5.7 Preprocessor directives

The preprocessor runs before the compiler proper.

Common directives:

- #include inserts declarations from a header.
- #define defines an object-like or function-like macro.
- #if, #ifdef, #ifndef, and #endif control conditional compilation.
- #undef removes a macro definition.

### Header guards

~~~c
#ifndef STUDENT_H
#define STUDENT_H

struct Student {
    int roll;
    double average;
};

#endif
~~~

A header guard prevents the same header contents from being included repeatedly in one translation unit.

### Macro hazards

~~~c
#define SQUARE(x) x * x
int result = SQUARE(1 + 2);
~~~

Textual expansion gives 1 + 2 * 1 + 2, which evaluates to 5, not 9. Parentheses help:

~~~c
#define SQUARE(x) ((x) * (x))
~~~

But calling SQUARE(i++) still increments i more than once. For ordinary computation, prefer a function:

~~~c
static inline int square_int(int x) {
    return x * x;
}
~~~

Macros do not obey normal function type checking and can evaluate arguments unexpectedly.

---

## 5.8 File streams

C standard I/O uses streams declared in stdio.h:

- stdin: standard input;
- stdout: standard output;
- stderr: standard error.

A file is opened with fopen, used through a FILE pointer, and closed with fclose.

~~~c
FILE *file = fopen("marks.txt", "r");
if (file == NULL) {
    perror("marks.txt");
    return 1;
}

/* read or write */

if (fclose(file) == EOF) {
    perror("close");
    return 1;
}
~~~

Always check whether fopen succeeded. Close files when finished to release resources and flush output.

### Common modes

| Mode | Meaning |
|---|---|
| r | Read existing text file |
| w | Write text; truncates an existing file |
| a | Append text |
| rb | Read binary |
| wb | Write binary; truncates existing file |
| ab | Append binary |

Modes with + allow both reading and writing, but file-position and update-stream rules must be followed.

### Character and line input

fgetc returns int, not char, so it can represent every unsigned-char value plus EOF.

~~~c
int ch;
while ((ch = fgetc(file)) != EOF) {
    putchar(ch);
}
~~~

Check for I/O errors separately when needed using ferror.

fgets reads at most size−1 characters and appends a null terminator when successful. It may preserve a newline.

### Formatted input

fscanf can parse formatted text but requires careful format specifiers and return-value checks. For untrusted or complex lines, read a bounded line first and parse it deliberately.

---

## 5.9 Text files versus binary files

A **text file** stores characters according to an encoding and may have platform-specific newline translation.

A **binary file** stores bytes without text interpretation by the C stream layer in binary mode. Binary data still needs a defined format for endianness, field widths, versioning, and padding.

Do not write a raw C structure to a portable file format and assume it is portable: structures may contain padding and machine-specific representations.

### Binary I/O sketch

~~~c
size_t written = fwrite(buffer, element_size, element_count, file);
if (written != element_count) {
    /* handle short write or error */
}
~~~

Read similarly with fread and verify the returned item count.

---

## 5.10 Command-line arguments

A common hosted C entry point is:

~~~c
int main(int argc, char *argv[]) {
    /* argc is argument count; argv is an array of argument strings */
    return 0;
}
~~~

- argv[0] commonly contains the program name.
- argv[1] through argv[argc−1] are user arguments, if present.
- argv[argc] is a null pointer by convention in the hosted environment.

Check argc before accessing an argument. Parse numeric arguments with robust functions such as strtol and validate the entire input and range.

---

## 5.11 Error handling and resource cleanup

C commonly reports failure using:

- return values;
- error codes;
- NULL pointers;
- EOF;
- errno for some library errors.

Check every operation that can fail: file open, allocation, conversion, read, write, and close. If a function acquires several resources, use a clear cleanup path so each acquired resource is released on every error path.

---

## 5.12 Worked examples

### A. Read integers and compute a sum

~~~c
#include <stdio.h>

int main(void) {
    FILE *file = fopen("numbers.txt", "r");
    if (file == NULL) {
        perror("numbers.txt");
        return 1;
    }

    long long total = 0;
    int value;
    while (fscanf(file, "%d", &value) == 1) {
        total += value;
    }

    if (ferror(file)) {
        perror("read");
        fclose(file);
        return 1;
    }

    if (fclose(file) == EOF) {
        perror("close");
        return 1;
    }

    printf("%lld\n", total);
    return 0;
}
~~~

The loop stops when parsing no longer succeeds. A production parser may need to distinguish normal end-of-file from malformed text rather than silently stopping at the first invalid token.

### B. Determine a union's size

~~~c
union Number {
    long long whole;
    double real;
    char bytes[16];
};
~~~

The size is at least 16 bytes because bytes has length 16, and may be larger if alignment requires it. It is not the sum of all members.

### C. Correct a macro problem

~~~c
#define MAX(a, b) ((a) > (b) ? (a) : (b))
~~~

Parentheses fix precedence problems, but MAX(i++, j++) may evaluate one argument more than once. A function-like macro cannot generally guarantee exactly-once evaluation in portable C; prefer a function when side effects are possible.

---

## 5.13 Questions and answers

1. **What is a structure?**  
   A user-defined aggregate with named members that may have different types.

2. **Why can sizeof a structure exceed the sum of member sizes?**  
   The implementation may insert alignment padding.

3. **How does a union differ from a structure?**  
   Structure members have distinct storage; union members share storage.

4. **Why use a tag with a union?**  
   To record which member is currently intended to be active.

5. **What does a header guard prevent?**  
   Repeated inclusion of the same header contents within a translation unit.

6. **Why can a macro square expression be wrong?**  
   Text substitution may change precedence or evaluate an argument more than once.

7. **Why does fgetc return int?**  
   It must represent all unsigned-char values plus the distinct EOF value.

8. **What does fopen return on failure?**  
   NULL.

9. **Why is writing a raw struct a poor portable file format?**  
   Padding, byte order, type widths, and floating representation may vary.

10. **What does argc represent?**  
    The count of command-line arguments passed to main.

### GATE/BPSC practice with solutions

1. Does a union of int, double, and char[20] use the sum of member sizes?  
   **Answer:** No. Its size is at least the largest member plus any alignment padding.

2. What may be printed by sizeof(struct { char c; int i; })?  
   **Answer:** An implementation-dependent value at least large enough for the members and their alignment; do not assume a portable exact size.

3. What does this macro produce for x=2?

~~~c
#define SQUARE(x) x*x
SQUARE(1 + x)
~~~

**Solution:** Expansion is 1+x*1+x. With x=2, the expression is 1+2*1+2 = 5, not 9.

4. Why is fgetc assigned to int?  
   **Answer:** EOF must be distinguishable from every possible byte value.

5. A program opens a file successfully but fails to close it and check output. What is the concern?  
   **Answer:** Buffered output may not be flushed successfully, and resources may remain unreleased.

### Multiple-choice check

1. Structure members generally have: A. separate storage B. all the same address C. no type D. no name  
2. Union members: A. share storage B. are always summed in size C. are separate processes D. are functions  
3. fopen failure is commonly indicated by: A. NULL B. EOF only C. zero bytes D. false filename  
4. fgetc returns: A. int B. FILE C. void D. double  
5. Header guards help prevent: A. repeated inclusion B. integer overflow C. disk failure D. recursion  
6. A raw struct file can be nonportable due to: A. padding and representations B. comments C. function names D. loops

**Answers:** 1-A, 2-A, 3-A, 4-A, 5-A, 6-A.

### Draw-from-memory task

Draw the memory difference between a struct with three fields and a union with the same fields. Then draw the C file-I/O lifecycle: open → check → read/write → check → close.
