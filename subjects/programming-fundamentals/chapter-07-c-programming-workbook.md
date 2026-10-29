# Chapter 7 — C Programming Workbook and Exam Practice

**Course:** Programming Fundamentals  
**Exam alignment:** GATE CS programming/recursion foundation; BPSC computer-science objective preparation  
**Prerequisite:** Chapters 1–6

This is an original practice set, not a reproduction of a past paper. Work each trace by hand before checking the explanation. For coding questions, state assumptions and input bounds before choosing a type.

---

## 7.1 A method for C output-tracing questions

1. Write the initial value and type of every variable.
2. Follow statements in execution order.
3. Track integer and floating-point operations separately.
4. Mark function calls and returns.
5. Check whether an expression has undefined behavior.
6. Do not guess output for undefined or unspecified behavior.
7. Use a trace table for loops and recursion.

| Step | Statement/condition | Variable values | Output |
|---|---|---|---|
| 1 | Initialization | ... | — |
| 2 | Condition | ... | ... |
| 3 | Body/update | ... | ... |

---

## 7.2 Fully worked trace problems

### Trace 1: integer quotient and remainder

~~~c
int a = 23, b = 6;
printf("%d %d\n", a / b, a % b);
~~~

**Answer:** 3 5. Check: 23 = 3×6 + 5.

### Trace 2: loop boundary

~~~c
int total = 0;
for (int i = 1; i < 5; i++) {
    total += i;
}
printf("%d\n", total);
~~~

**Trace:** total becomes 1, 3, 6, and 10. The next condition i<5 is false.  
**Answer:** 10.

### Trace 3: triangular nested loop

~~~c
int count = 0;
for (int i = 1; i <= 4; i++) {
    for (int j = 0; j < i; j++) {
        count++;
    }
}
printf("%d\n", count);
~~~

**Solution:** Inner loop counts are 1+2+3+4 = 10.  
**Answer:** 10.

### Trace 4: argument copy

~~~c
void update(int x) {
    x = x + 10;
}

int main(void) {
    int value = 2;
    update(value);
    printf("%d\n", value);
}
~~~

**Answer:** 2. The parameter is a copy.

### Trace 5: pointer update

~~~c
void update(int *x) {
    *x = *x + 10;
}
~~~

If value is 2 and the call is update(&value), value becomes 12 because the function accesses the caller's object through its address.

### Trace 6: pointer increment

~~~c
int a[] = {2, 4, 6};
int *p = a;
printf("%d", *p++);
printf("%d\n", *p);
~~~

**Answer:** 24. The first expression reads a[0], then advances p to a[1]. The second reads 4. Separate output calls avoid argument evaluation-order ambiguity.

### Trace 7: static local variable

~~~c
int next(void) {
    static int count = 0;
    return ++count;
}
~~~

If main calls next three times and prints each result on a separate line, the output is 1, then 2, then 3. The object persists between calls.

### Trace 8: recursion output

~~~c
void show(int n) {
    if (n == 0) return;
    printf("%d", n);
    show(n - 1);
    printf("%d", n);
}
~~~

**Answer for show(2):** 2112. Descending calls print 2 and 1; returning calls print 1 and 2.

### Trace 9: short circuit

~~~c
int x = 0;
if (x != 0 && 10 / x > 1) {
    puts("yes");
} else {
    puts("no");
}
~~~

**Answer:** no. The left side of && is false, so division is skipped.

### Trace 10: do-while

~~~c
int i = 5;
do {
    printf("%d", i);
    i++;
} while (i < 5);
~~~

**Answer:** 5. The body runs once before the condition is checked.

### Trace 11: undefined behavior

~~~c
int i = 1;
printf("%d %d\n", i++, i++);
~~~

**Answer:** No portable output is defined. The object is modified more than once in a full expression without the required sequencing. Do not guess a compiler-specific result.

---

## 7.3 Short-answer review

1. Why does assigning a function parameter not modify the caller's int?  
   **Answer:** The parameter is a separate copy because C passes values.

2. When can a pointer be one past an array?  
   **Answer:** As an end marker one element past the last array element; it cannot be dereferenced.

3. Why is sizeof on an array parameter not the full array size?  
   **Answer:** The parameter is adjusted to a pointer.

4. Why does a C string need a null terminator?  
   **Answer:** String functions use it to identify where the sequence ends.

5. What does a recursive function need to show termination?  
   **Answer:** A reachable base case and a measure that decreases toward it.

6. What does static local storage preserve?  
   **Answer:** The local object's value across function calls.

7. Why can realloc require a temporary pointer?  
   **Answer:** On failure the original allocation remains valid; assigning directly could lose its only pointer.

8. Why must a file-open result be checked?  
   **Answer:** Opening can fail because of a bad path, permissions, or storage state.

---

## 7.4 Multiple-choice exam set

Choose one answer. Check explanations after attempting all questions.

1. C integer division 23/6 gives:  
   A. 3  B. 3.5  C. 4  D. 5

2. What is the value of 23%6 for positive int operands?  
   A. 3  B. 4  C. 5  D. 6

3. Which statement about C argument passing is correct?  
   A. All arguments are passed by value  
   B. Every array is copied with its length  
   C. Integers are passed by reference  
   D. Only pointers are passed

4. For an array of n elements, valid indexes are:  
   A. 1 through n  B. 0 through n−1  C. 0 through n  D. −1 through n−1

5. Pointer arithmetic on an int pointer advances by:  
   A. one byte always  B. sizeof(int) bytes  C. one array  D. an arbitrary amount

6. Which declaration means pointer to an array of five int?  
   A. int *p[5]  B. int (*p)[5]  C. int p[5]*  D. int** p

7. A one-past pointer:  
   A. may be formed but not dereferenced  
   B. is always NULL  
   C. points to a valid extra element  
   D. must be freed

8. Which function compares C string contents?  
   A. strcmp  B. sizeof  C. strlen only  D. printf

9. What does strlen count?  
   A. Bytes including terminator  
   B. Characters before the null terminator  
   C. Array capacity  
   D. Unicode graphemes in every case

10. Which operation can cause undefined behavior?  
    A. Reading a valid array element  
    B. Dereferencing an uninitialised pointer  
    C. Comparing an int with zero  
    D. Calling a function with an int argument

11. For an unsigned N-bit integer, arithmetic wraps modulo:  
    A. N  B. 2N  C. 2^N  D. N^2

12. Signed integer overflow in C is:  
    A. guaranteed wraparound  B. undefined behavior  C. guaranteed saturation  D. a linker error

13. Which object has static storage duration?  
    A. A static local object  B. An ordinary local object  C. A parameter  D. A loop counter

14. A function prototype declares:  
    A. interface information for a function  B. a file format  C. array runtime length  D. a heap block

15. The expression a[i] is defined in terms of:  
    A. *(a+i)  B. *a+i  C. &(a+i)  D. a×i

16. realloc failure for a nonzero requested size returns:  
    A. NULL while the old allocation remains valid  
    B. a pointer to freed storage  
    C. the old size  
    D. EOF

17. A struct may be larger than the sum of its members because of:  
    A. alignment padding  B. recursion  C. function calls  D. string comparison

18. Union members:  
    A. share storage  B. always use separate full storage  C. must have the same type  D. are always arrays

19. fgetc returns int so it can represent:  
    A. all byte values plus EOF  B. only signed char values  C. a FILE pointer  D. line count only

20. Which loop executes its body at least once?  
    A. do-while  B. while  C. for with false condition  D. none

21. Which keyword makes a local object retain its value between calls?  
    A. static  B. extern  C. auto  D. register

22. A safe function for reading a bounded line into a character array is commonly:  
    A. fgets  B. gets  C. strcat  D. unbounded scanf %s

23. The conventional factorial base case is:  
    A. 0! = 1  B. 0! = 0  C. 1! = 0  D. factorial is undefined for zero

24. Naive recursive Fibonacci takes approximately:  
    A. constant time  B. logarithmic time  C. exponential time  D. linear time in all implementations

25. If a function needs to change the caller's int, it should commonly receive:  
    A. pointer to int  B. copied int only  C. float  D. string literal

### Answer key with explanations

1. A — integer division truncates toward zero.  
2. C — 23 = 3×6 + 5.  
3. A — C passes every argument by value; a pointer can designate caller data.  
4. B — zero-based indexes end at n−1.  
5. B — pointer arithmetic scales by the pointed-to type.  
6. B — parentheses make p a pointer before applying the array size.  
7. A — one-past is an end marker, not an element.  
8. A — strcmp compares character sequences.  
9. B — strlen stops before the null terminator; it does not count grapheme clusters.  
10. B — an indeterminate pointer cannot safely be dereferenced.  
11. C — unsigned N-bit arithmetic is modulo 2^N.  
12. B — C does not promise signed wraparound.  
13. A — static local storage lasts for the program execution.  
14. A — a prototype gives return and parameter types to the compiler.  
15. A — indexing is defined through pointer addition and dereference.  
16. A — on failure, the original allocation remains valid.  
17. A — member alignment may add padding.  
18. A — union members overlap in storage.  
19. A — EOF must be distinguishable from every byte value.  
20. A — the condition is checked after the first execution.  
21. A — static local objects retain state.  
22. A — fgets accepts the buffer size and bounds the read.  
23. A — this base case makes the recursive definition terminate correctly.  
24. C — the simple recurrence repeats subproblems exponentially.  
25. A — a pointer lets the function access the caller's object.

---

## 7.5 Coding practice with solution outlines

For each problem, write a contract, choose a type, handle edge cases, then state complexity.

### A. Reverse the digits of a non-negative integer

Repeatedly take the last digit with remainder 10, append it to the answer by multiplying by 10, and remove it by integer division by 10. Check for overflow if the result may exceed the type range.

### B. Test primality

Reject n<2. Test divisors from 2 while divisor×divisor<=n. If any divides n, it is composite; otherwise it is prime. Ensure divisor×divisor cannot overflow for the stated input bound.

### C. Compute GCD

Repeatedly replace (a,b) by (b,a%b) until b=0. For non-negative inputs, the remaining a is the GCD.

### D. Count characters in a string

Traverse until the null terminator and increment when the character matches. State whether comparison is case-sensitive and whether input is ASCII-only.

### E. Transpose a square matrix in place

For each i, swap a[i][j] with a[j][i] only when j>i. This avoids swapping each pair twice. Time O(n²), extra space O(1).

### F. Read a line safely

Use fgets into a fixed-size buffer. Detect whether the newline was read; if not, the line may have been truncated and the remainder must be handled.

### G. Recursive sum of an array

Define sum(a,n)=0 when n=0; otherwise sum(a,n)=a[n−1]+sum(a,n−1). Time O(n), stack O(n). Compare with an iterative O(1)-extra-space version.

### H. Allocate n integers and find the maximum

Check multiplication overflow before allocating n*sizeof(int); check allocation result; define behavior for n=0; initialise maximum only when n>0; free on every path.

---

## 7.6 Common exam traps

- Treating integer division as floating-point division.
- Forgetting that C arrays are zero-indexed.
- Assuming array length travels with a pointer.
- Evaluating unsequenced side effects as if C guaranteed a fixed order.
- Assuming signed overflow wraps.
- Forgetting string terminators and buffer capacity.
- Returning a pointer to an automatic local array.
- Counting a duplicate maximum as the second-largest distinct value.
- Ignoring recursion stack depth.
- Assuming sizeof has the same result for an array and a pointer.
- Treating a compiler's output as the C standard when behavior is undefined.

---

## 7.7 Final self-assessment

You are ready to move to Data Structures and Algorithms when you can:

- explain C pass-by-value and use pointers for outputs;
- trace nested loops, recursion, and array-pointer expressions;
- calculate two-dimensional row-major addresses;
- distinguish valid, one-past, null, uninitialised, and dangling pointers;
- allocate, check, use, and release memory;
- explain why a C expression is undefined rather than guessing its output;
- solve the coding practice problems and test boundary cases.

**Next course:** Data Structures and Algorithms. Arrays as language-level storage were covered here; stacks, queues, linked lists, trees, heaps, graphs, searching, sorting, and design techniques are developed there.
