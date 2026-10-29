# Chapter 4 — Pointers, Memory, and Dynamic Allocation in C

**Course:** Programming Fundamentals  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–3

## What you will learn

You will read pointer declarations, follow addresses and values, use pointer arithmetic, distinguish arrays from pointers, pass objects by address, allocate and release dynamic memory, and avoid common undefined behavior.

---

## 4.1 Objects, addresses, and pointers

A pointer object stores an address. Its type says what kind of object it may designate and how pointer arithmetic and dereferencing behave.

~~~c
int value = 42;
int *pointer = &value;
~~~

- `value` is an int object.
- `&value` means the address of that object.
- `pointer` stores the address.
- `*pointer` accesses the int at the stored address; this is called dereferencing.

~~~text
pointer
  |
  v
+---------+
| value 42|
+---------+
 address p
~~~

Pointer variables are objects too, so they have their own addresses.

---

## 4.2 Declaration reading

Read a declaration from the identifier outward, paying attention to parentheses.

~~~c
int *p;       /* p is a pointer to int */
int **pp;     /* pp is a pointer to pointer to int */
int a[4];     /* a is an array of 4 int */
int *a2[4];   /* a2 is an array of 4 pointers to int */
int (*p2)[4]; /* p2 is a pointer to an array of 4 int */
~~~

Parentheses change binding. Without parentheses, the array brackets bind more tightly than the pointer star.

### Array of pointers versus pointer to array

~~~mermaid
flowchart LR
    A[Array of 4 pointers] --> P0[pointer 0]
    A --> P1[pointer 1]
    A --> P2[pointer 2]
    A --> P3[pointer 3]
    Q[Pointer to an array of 4 int] --> R[one contiguous 4-int array]
~~~

Use examples rather than relying on visual memory:

- `int *p[4]`: p is an array; each element is a pointer to int.
- `int (*p)[4]`: p is a pointer; it points to one array of four int.

---

## 4.3 Null, uninitialised, and dangling pointers

- **Null pointer:** points to no object. Use NULL from stddef.h or a null pointer constant.
- **Uninitialised pointer:** has an indeterminate value; reading or dereferencing it is unsafe.
- **Dangling pointer:** once pointed to a live object, but that object's lifetime has ended.
- **Wild pointer:** informal term for an uninitialised or invalid pointer.
- **Void pointer:** generic object pointer type; it must be converted to an appropriate object-pointer type before dereferencing.

~~~c
int *p = NULL;
if (p != NULL) {
    printf("%d\n", *p);
}
~~~

The condition prevents dereferencing a null pointer. It cannot prove that a non-null pointer is valid; validity depends on how it was obtained and whether the object is still alive.

---

## 4.4 Pointer arithmetic

For a pointer p to an element of an array, p+1 points to the next element, not necessarily the next byte. The address increment is sizeof the pointed-to type.

~~~c
int a[4] = {10, 20, 30, 40};
int *p = a;
~~~

~~~text
p       points to a[0]
p + 1   points to a[1]
*(p+2)  is a[2], value 30
~~~

Pointer arithmetic is defined only within the same array object or one position past its end. A one-past pointer may be compared or used as an end iterator but must not be dereferenced.

~~~c
for (int *current = a; current < a + 4; current++) {
    printf("%d\n", *current);
}
~~~

Do not form a pointer far outside an array and then assume it is safe just because you will not dereference it.

---

## 4.5 Relationship between arrays and pointers

In most expressions, an array expression is converted to a pointer to its first element. This conversion is often called **array decay**, but an array is not itself a pointer.

Important differences:

- An array has storage for all its elements; a pointer stores one address.
- sizeof array gives total array size in its defining scope; sizeof pointer gives pointer-object size.
- An array cannot be assigned as a whole with ordinary assignment; a pointer can be reassigned.
- Applying unary & to an array yields a pointer to the whole array, which has a different type from pointer-to-first-element.

~~~c
int a[5];
int *p = a;       /* pointer to first element */
int (*whole)[5] = &a; /* pointer to the entire array */
~~~

Both addresses may have the same numeric representation, but pointer arithmetic differs: p+1 advances one int; whole+1 advances five ints.

---

## 4.6 Pointer parameters and output values

To modify a caller's value, pass a pointer:

~~~c
void increment(int *value) {
    if (value != NULL) {
        (*value)++;
    }
}
~~~

Parentheses matter. `(*value)++` increments the pointed-to integer. `*value++` increments the pointer itself because postfix ++ binds more tightly.

### Pointer to pointer

A pointer-to-pointer can allow a function to update the caller's pointer:

~~~c
int make_value(int **out) {
    if (out == NULL) {
        return 0;
    }
    *out = malloc(sizeof **out);
    if (*out == NULL) {
        return 0;
    }
    **out = 25;
    return 1;
}
~~~

The caller must eventually free the allocation. In a simple example, an output pointer can also be passed directly as a parameter and returned with a status.

---

## 4.7 const with pointers

~~~c
const int *p1;       /* pointer to const int: cannot modify the int through p1 */
int *const p2 = &x;  /* const pointer: p2 cannot point elsewhere */
const int *const p3 = &x; /* const pointer to const int */
~~~

Read from the identifier outward:

- `const int *p`: pointer can change; pointed-to int cannot be modified through p.
- `int *const p`: pointer cannot change; pointed-to int may be modified.
- `const int *const p`: neither can change through p.

const restricts this access path; another non-const pointer may still modify the object if the object itself is not declared const.

---

## 4.8 Dynamic memory

Dynamic allocation requests memory at runtime from the heap-like allocation area.

Include stdlib.h:

- `malloc(size)` allocates size bytes; contents are indeterminate.
- `calloc(count, size)` allocates space for an array and initializes all bytes to zero.
- `realloc(pointer, new_size)` changes an allocation size, possibly moving it.
- `free(pointer)` releases an allocation previously returned by an allocation function.

### Safe allocation pattern

~~~c
#include <stdlib.h>

size_t n = 100;
int *items = malloc(n * sizeof *items);
if (items == NULL) {
    /* handle allocation failure */
    return 1;
}

/* use items[0] through items[n-1] */

free(items);
items = NULL;
~~~

Using `sizeof *items` ties the allocation to the pointed-to type and is safer than repeating the type name.

In production code, check that n×sizeof(*items) does not overflow size_t before allocation when n comes from external input.

### realloc safely

Do not overwrite the only pointer with realloc's result until success is checked:

~~~c
int *temporary = realloc(items, new_n * sizeof *items);
if (temporary == NULL) {
    /* items remains valid; handle failure */
} else {
    items = temporary;
}
~~~

If new size is zero, behavior has portability subtleties; handle zero-size cases explicitly.

---

## 4.9 Ownership, leaks, and use-after-free

Every successful allocation needs a clear owner responsible for releasing it.

- **Memory leak:** allocated memory is no longer needed but is never freed.
- **Double free:** freeing the same allocation twice.
- **Use-after-free:** accessing an allocation after it was freed.
- **Buffer overflow:** writing beyond an allocated or declared object.
- **Ownership transfer:** one component gives responsibility for freeing the object to another.

~~~c
int *p = malloc(sizeof *p);
if (p == NULL) {
    return 1;
}
*p = 9;
free(p);
p = NULL; /* clears this variable; other aliases would still be dangling */
~~~

Setting one pointer to NULL does not repair other pointers that alias the same freed memory.

---

## 4.10 Function pointers

A function pointer stores the address of a function with a compatible signature.

~~~c
int add(int a, int b) {
    return a + b;
}

int (*operation)(int, int) = add;
int result = operation(3, 4);
~~~

Function pointers support callbacks and tables of operations. They are different from object pointers and should not be dereferenced like data pointers.

---

## 4.11 Pointer and array example

~~~c
int a[] = {4, 7, 9};
int *p = a;

printf("%d\n", *p);       /* 4 */
printf("%d\n", *(p + 1)); /* 7 */
printf("%d\n", p[2]);     /* 9; equivalent to *(p + 2) */
~~~

For an array expression a and valid index i:

~~~text
a[i] is defined in terms of *(a + i)
~~~

This equivalence does not make out-of-range indexing safe.

---

## 4.12 Common undefined or unsafe patterns

~~~c
int *p;       /* uninitialised */
printf("%d", *p); /* invalid: indeterminate pointer */
~~~

~~~c
int *p = malloc(sizeof *p);
free(p);
printf("%d", *p); /* use-after-free */
~~~

~~~c
int a[3];
int *one_past = a + 3;
/* one_past is allowed as an end marker, but *one_past is invalid */
~~~

Other unsafe cases include returning the address of a local automatic object, arithmetic beyond the same array, writing to a string literal, and forgetting to check allocation failure.

---

## 4.13 Worked pointer traces

**1. Trace pointer arithmetic**

~~~c
int a[] = {5, 8, 11};
int *p = a + 1;
printf("%d %d\n", *p, p[1]);
~~~

**Solution:** p points at a[1], value 8. p[1] means *(p+1), which is a[2], value 11. Output: 8 11.

**2. Trace the swap**

~~~c
int x = 3, y = 9;
swap_int(&x, &y);
~~~

The function receives addresses. temporary gets 3, *a becomes 9, *b becomes 3. After return x=9, y=3.

**3. Identify the type**

~~~c
int values[6];
int *p = values;
int (*q)[6] = &values;
~~~

p is pointer to int. q is pointer to an array of six int. Both may point to the same starting address, but p+1 advances one int while q+1 advances a full six-int array.

---

## 4.14 Questions and answers

1. **What does a pointer store?**  
   An address that can designate an object or function, depending on pointer type.

2. **What does dereferencing do?**  
   Accesses the object designated by a valid pointer.

3. **How does p+1 work for an int pointer?**  
   It advances to the next int element, scaled by sizeof(int).

4. **Can a one-past pointer be dereferenced?**  
   No. It may be formed as an end marker but does not designate an array element.

5. **Are arrays and pointers identical in C?**  
   No. Arrays have element storage; pointers are separate objects that store addresses.

6. **What does malloc initialize?**  
   It allocates bytes but leaves their contents indeterminate.

7. **What does calloc do differently?**  
   It initializes the allocated bytes to all-bits-zero.

8. **What is a memory leak?**  
   An allocation remains unreleased after the program has lost the ability or intention to free it.

9. **Why use a temporary pointer with realloc?**  
   If reallocation fails, the original pointer remains valid and can still be freed.

10. **What is a dangling pointer?**  
    A pointer that no longer designates a live object.

### GATE/BPSC practice with solutions

1. Given int a[5] and int *p=a, what does p+3 designate?  
   **Answer:** a[3], the fourth element.

2. Is a+5 valid for int a[5]?  
   **Answer:** It may be formed as a one-past pointer, but it must not be dereferenced.

3. What does `sizeof(a)` return when a is a 5-element int array in the same scope?  
   **Answer:** 5×sizeof(int) bytes.

4. What does `sizeof(p)` return when p is a pointer to int?  
   **Answer:** The size of the pointer object, which is implementation-dependent.

5. Which is safer for allocation: `malloc(n * sizeof(int))` or `malloc(n * sizeof *p)` when p is int*?  
   **Answer:** sizeof *p reduces type repetition and remains correct if p's pointed-to type changes.

6. A function allocates an array and returns its pointer. When is that valid?  
   **Answer:** If the allocation remains alive after return and ownership/lifetime is clearly documented; returning a pointer to a local array is not valid.

### Multiple-choice check

1. `int *p[4]` means: A. array of four int pointers B. pointer to 4-int array C. 4-int scalar D. function  
2. `int (*p)[4]` means: A. array of pointers B. pointer to array of four int C. int pointer D. 4 arrays  
3. Pointer arithmetic scales by: A. one byte always B. pointed-to type size C. address digit count D. array length  
4. malloc returns: A. an allocated pointer or NULL B. initialized ints only C. a file handle D. a stack frame  
5. A one-past pointer may be: A. used as an end marker B. dereferenced C. freed if not allocated D. written through  
6. A safe realloc pattern stores the result first in: A. a temporary pointer B. a global C. an int D. a char array

**Answers:** 1-A, 2-B, 3-B, 4-A, 5-A, 6-A.

### Draw-from-memory task

Draw three int objects in a contiguous array with addresses; show a pointer moving by one element. Then draw an allocated block and mark its owner, lifetime, and free operation.
