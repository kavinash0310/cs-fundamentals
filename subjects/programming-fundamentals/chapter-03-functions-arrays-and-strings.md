# Chapter 3 — Functions, Arrays, and Strings in C

**Course:** Programming Fundamentals  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–2

## What you will learn

You will declare and define functions, explain C's pass-by-value rule, reason about scope and lifetime, traverse one- and two-dimensional arrays, understand how arrays are passed to functions, and process null-terminated strings safely.

---

## 3.1 Functions and modular design

A **function** is a named block of code that performs a clear task. Functions help divide a large problem into manageable, testable parts.

A function has a return type, name, parameter list, and body. Its **prototype** tells the compiler its interface before a call is checked.

~~~c
int square(int value); /* declaration/prototype */

int square(int value) { /* definition */
    return value * value;
}

int main(void) {
    int answer = square(7);
    printf("%d\n", answer);
    return 0;
}
~~~

The prototype and definition must agree in compatible types. A function returning no value uses return type void. A function that returns a value should return it on every path where a result is required.

### Design a function contract

For each function, state its purpose, parameters and valid ranges, return value, side effects, failure behavior, preconditions, and postconditions.

**Example:** `int max_of_two(int a, int b)` returns the greater value. If a equals b, either argument is the maximum.

---

## 3.2 C passes arguments by value

C always passes function arguments by value: the parameter receives a copy of the argument value. To let a function modify a caller's object, pass its address using a pointer.

~~~c
void set_to_zero(int x) {
    x = 0; /* changes only local parameter copy */
}

void set_to_zero_at(int *x) {
    if (x != NULL) {
        *x = 0; /* changes the caller's object */
    }
}
~~~

~~~c
int value = 9;
set_to_zero(value);       /* value remains 9 */
set_to_zero_at(&value);   /* value becomes 0 */
~~~

### Swap example

~~~c
void swap_int(int *a, int *b) {
    int temporary = *a;
    *a = *b;
    *b = temporary;
}
~~~

The pointer values themselves are copied, but both copied pointers designate the caller's integers.

---

## 3.3 Return values and output parameters

A function may return one value directly. To report multiple results, common approaches include returning a struct, using pointer parameters for output values, or returning a status and writing results through pointers.

~~~c
int divide_with_remainder(int dividend, int divisor,
                          int *quotient, int *remainder) {
    if (divisor == 0 || quotient == NULL || remainder == NULL) {
        return 0; /* failure */
    }
    *quotient = dividend / divisor;
    *remainder = dividend % divisor;
    return 1; /* success */
}
~~~

Check the status before using output values. The error convention should be documented.

C functions cannot return an array type directly. They can return a pointer, but the pointed-to object's lifetime must remain valid after the function returns.

---

## 3.4 Scope, storage duration, and linkage

These terms describe different properties:

- **Scope:** where a name can be used in source code.
- **Storage duration:** how long an object's storage exists.
- **Linkage:** whether declarations in different places refer to the same entity.

### Block scope and automatic duration

~~~c
void f(void) {
    int local = 5; /* block scope, automatic storage duration */
}
~~~

The local variable's lifetime normally ends when the block exits.

### Static local object

~~~c
int next_count(void) {
    static int count = 0;
    count++;
    return count;
}
~~~

The name count has block scope, but its object has static storage duration and retains its value between calls.

### File-scope names

A file-scope declaration is visible from its declaration to the end of the file, subject to declarations and linkage.

- A file-scope object or function commonly has external linkage by default.
- The static keyword at file scope gives internal linkage, limiting direct name visibility to that translation unit.
- extern declares a name whose definition is expected elsewhere.

Do not confuse static storage duration with the static keyword's effect on linkage; context matters.

---

## 3.5 Arrays and indexing

An array is a contiguous sequence of elements of one element type.

~~~c
int marks[5] = {72, 81, 66, 90, 75};
~~~

Valid indexes are 0 through 4. C does not automatically check bounds.

- `marks[0]` is the first element.
- `marks[i]` accesses element i.
- `sizeof marks` in the defining scope gives the array's total size in bytes.
- `sizeof marks[0]` gives one element's size.
- The element count can be computed as `sizeof marks / sizeof marks[0]` while marks is still an array.

~~~c
size_t count = sizeof marks / sizeof marks[0];
~~~

This formula fails after the array is passed to a function because the parameter is adjusted to a pointer.

### Partial initialization

~~~c
int values[5] = {3, 8};
~~~

The first two elements are 3 and 8; remaining elements are zero-initialized. An uninitialised automatic array has indeterminate elements. Do not read them.

---

## 3.6 Arrays as function parameters

When an array parameter is declared, it is adjusted to a pointer parameter.

~~~c
int sum_array(const int a[], size_t n) {
    int total = 0;
    for (size_t i = 0; i < n; i++) {
        total += a[i];
    }
    return total;
}
~~~

The declaration is adjusted to a pointer to const int; the function receives no automatic array length. Pass n separately.

`const int a[]` means the function will not modify elements through a. It does not make a deep copy.

**Common error:**

~~~c
void wrong_size(int a[]) {
    printf("%zu\n", sizeof a); /* size of a pointer, not original array */
}
~~~

The array-to-pointer conversion does not occur in some contexts, including applying sizeof directly to an array or applying unary &. These exceptions are why sizeof on the original array can return its full size.

---

## 3.7 Traversal patterns

### Sum and average

~~~c
long long sum = 0;
for (size_t i = 0; i < n; i++) {
    sum += a[i];
}
double average = (n > 0) ? (double)sum / n : 0.0;
~~~

The code must define behavior for n=0. A production function should report that no average exists rather than silently inventing one.

### Minimum and maximum

For n>0, initialize from the first element rather than from zero; this works for all-negative arrays.

~~~c
int maximum = a[0];
for (size_t i = 1; i < n; i++) {
    if (a[i] > maximum) {
        maximum = a[i];
    }
}
~~~

### Reverse in place

~~~c
size_t left = 0;
size_t right = n;
while (left < right) {
    right--;
    if (left >= right) {
        break;
    }
    int temporary = a[left];
    a[left] = a[right];
    a[right] = temporary;
    left++;
}
~~~

The right index is initially one past the last element. It is decremented only after the loop confirms the range is nonempty, avoiding unsigned underflow. Trace n=0, n=1, n=2, and n=5.

---

## 3.8 Multidimensional arrays and row-major layout

~~~c
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
~~~

C stores this array in **row-major order**:

~~~text
1, 2, 3, 4, 5, 6
~~~

For a row-major array with R rows and C columns, the linear element index of matrix[i][j] is:

~~~text
i × C + j
~~~

If the element size is s bytes and base address is B:

~~~text
address(matrix[i][j]) = B + (i×C + j)×s
~~~

**Worked address example:** Base address 1000, element size 4 bytes, 3 columns; find address of matrix[1][2].

~~~text
Linear index = 1×3 + 2 = 5
Address = 1000 + 5×4 = 1020
~~~

An array parameter for a 2D array must communicate the column count so the compiler can compute row offsets:

~~~c
void print_matrix(size_t rows, int a[][3]) {
    for (size_t i = 0; i < rows; i++) {
        for (size_t j = 0; j < 3; j++) {
            printf("%d ", a[i][j]);
        }
        putchar('\n');
    }
}
~~~

The first dimension can be omitted in this function parameter form; later dimensions are needed for address calculation.

---

## 3.9 Strings in C

C has no built-in string object type. A **C string** is a character array whose content ends at a null character `'\0'`.

~~~c
char word[] = "cat";
~~~

The array contains four chars: 'c', 'a', 't', and '\0'. Its size is 4.

~~~text
Index:  0   1   2   3
Value: 'c' 'a' 't' '\0'
~~~

A character constant uses single quotes; a string literal uses double quotes.

~~~c
char letter = 'A';
char word[] = "A";
~~~

The first is one character object. The second is a two-character array containing A and the terminating null character.

### String literal and modifiability

~~~c
char editable[] = "cat";
editable[0] = 'b'; /* valid: array contains a modifiable copy */

const char *literal = "cat";
/* literal[0] = 'b'; modifying a string literal has undefined behavior */
~~~

Treat string literals as non-modifiable.

### Safe input

~~~c
char name[40];
if (fgets(name, sizeof name, stdin) != NULL) {
    /* name contains at most 39 input characters plus the terminator */
}
~~~

fgets limits input and terminates the resulting string. It may keep the newline if one fits; remove it deliberately if needed.

scanf with %s stops at whitespace. If used, specify a field width smaller than the destination buffer:

~~~c
char name[40];
if (scanf("%39s", name) == 1) {
    /* one whitespace-delimited token */
}
~~~

Never use an unbounded %s into a fixed-size buffer.

---

## 3.10 Standard string functions

Include string.h for functions such as:

- strlen(s): number of characters before the null terminator; does not count '\0'.
- strcmp(a, b): compares strings lexicographically; returns negative, zero, or positive.
- strcpy(destination, source): copies including terminator; destination must be large enough.
- strncpy: has edge cases and may not null-terminate when truncated; use with care.
- strcat(destination, source): appends; destination must have enough space.
- strchr(s, c): searches for a character.

Do not compare strings using ==; that compares pointer values in common expressions, not the character contents.

~~~c
if (strcmp(answer, "yes") == 0) {
    puts("confirmed");
}
~~~

For safer application code, validate buffer sizes and consider bounded alternatives or higher-level string abstractions.

---

## 3.11 Common mistakes

- Accessing a[i] when i >= n or i < 0.
- Reading an uninitialised array element.
- Assuming an array parameter knows the original array length.
- Applying sizeof to a pointer and expecting the caller's array size.
- Forgetting the null terminator when constructing a C string.
- Comparing two strings with == instead of strcmp.
- Writing into a string literal.
- Passing a too-small destination to strcpy or strcat.
- Treating characters as one byte in every text encoding; C char is a byte, but a Unicode character may use multiple bytes.
- Returning a pointer to an automatic local array after the function ends.

---

## 3.12 Worked examples

### A. Count occurrences

~~~c
size_t count_value(const int a[], size_t n, int target) {
    size_t count = 0;
    for (size_t i = 0; i < n; i++) {
        if (a[i] == target) {
            count++;
        }
    }
    return count;
}
~~~

Time is O(n); extra space is O(1).

### B. Find the second-largest distinct value

Assume at least two distinct input values. Use flags so the algorithm does not depend on a magic starting value.

~~~c
int second_largest(const int a[], size_t n, int *result) {
    if (n < 2 || result == NULL) {
        return 0;
    }

    int largest = a[0];
    int second = 0;
    int have_second = 0;

    for (size_t i = 1; i < n; i++) {
        if (a[i] > largest) {
            second = largest;
            have_second = 1;
            largest = a[i];
        } else if (a[i] < largest &&
                   (!have_second || a[i] > second)) {
            second = a[i];
            have_second = 1;
        }
    }

    if (!have_second) {
        return 0;
    }
    *result = second;
    return 1;
}
~~~

For [5, 5, 3], the result is 3, not 5. Time O(n), extra space O(1).

### C. Reverse a string in place

~~~c
#include <string.h>

void reverse_string(char s[]) {
    size_t length = strlen(s);
    for (size_t left = 0; left < length / 2; left++) {
        size_t right = length - 1 - left;
        char temporary = s[left];
        s[left] = s[right];
        s[right] = temporary;
    }
}
~~~

This reverses bytes before the null terminator. It is appropriate for ASCII-like input; reversing UTF-8 bytes can break multibyte characters.

---

## 3.13 Questions and answers

1. **How does C pass function arguments?**  
   By value. Pointer values can be passed to let a function modify caller-owned objects.

2. **What is a function prototype?**  
   A declaration that tells the compiler the function's return and parameter types.

3. **What is scope?**  
   Where a name can be used in source code.

4. **What does static local storage do?**  
   It preserves an object's value between calls while the name remains local to its block.

5. **What are valid indexes for an n-element C array?**  
   0 through n−1.

6. **Does a C array parameter carry its length?**  
   No. The parameter is adjusted to a pointer; pass the length separately.

7. **What does sizeof array return in the defining scope?**  
   Total bytes occupied by the array, before it decays to a pointer.

8. **What is the row-major linear index of a[i][j] in a C-column array?**  
   i×C+j.

9. **How does a C string end?**  
   With a null character, '\0'.

10. **Why use strcmp instead of == to compare string contents?**  
    strcmp compares characters; == commonly compares pointer values.

### GATE/BPSC practice with solutions

1. What is printed?

~~~c
void change(int x) { x = 8; }

int main(void) {
    int a = 3;
    change(a);
    printf("%d\n", a);
}
~~~

**Answer:** 3. The parameter is a copy.

2. What is printed?

~~~c
int a[4] = {2, 4, 6, 8};
printf("%zu\n", sizeof a / sizeof a[0]);
~~~

**Answer:** 4 in the scope where a is an array.

3. If base address is 200, element size is 4, and a two-dimensional array has 5 columns, find address of a[2][3].  
**Solution:** linear index 2×5+3=13; address 200+13×4=252.

4. What is strlen("cat")?  
**Answer:** 3. The terminator is not counted.

5. What does strcmp return for identical strings?  
**Answer:** 0.

6. Why is returning a pointer to a local array invalid?  
**Answer:** The local object's lifetime ends when the function returns, so the pointer no longer designates a live object.

### Multiple-choice check

1. C passes an int argument: A. by value B. by reference automatically C. as a global D. only by pointer  
2. An array with 6 elements has valid indexes: A. 1..6 B. 0..5 C. 0..6 D. 1..5  
3. C strings end in: A. space B. newline C. null character D. EOF  
4. To compare string contents use: A. strcmp B. == C. sizeof D. &  
5. A 2D array with 4 columns stores a[i][j] at linear index: A. i+j B. i×4+j C. i×j D. 4×i×j  
6. When an array is passed to a function, the parameter is generally: A. adjusted to pointer B. copied with its length C. converted to a struct D. automatically bounds-checked

**Answers:** 1-A, 2-B, 3-C, 4-A, 5-B, 6-A.

### Draw-from-memory task

Draw an array's contiguous memory, label indexes and addresses, then draw the null terminator of the string cat. Explain what changes when the array parameter enters a function.
