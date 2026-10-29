# Chapter 2 — Expressions, Control Flow, and Program Tracing in C

**Course:** Programming Fundamentals  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapter 1

## What you will learn

You will use selection and iteration constructs, evaluate C expressions safely, trace control flow with tables, identify common precedence and loop errors, and reason about termination.

---

## 2.1 Boolean conditions in C

C does not require a separate Boolean type for conditions. Zero is false; any nonzero scalar value is true. C99 also provides _Bool and, through stdbool.h, the names bool, true, and false.

~~~c
#include <stdbool.h>

bool is_even = (number % 2 == 0);
if (is_even) {
    puts("even");
}
~~~

A comparison such as x < y produces 0 or 1. Prefer explicit comparisons and Boolean variables because they make intent clear.

---

## 2.2 if, else, and nested decisions

~~~c
if (marks >= 80) {
    grade = 'A';
} else if (marks >= 60) {
    grade = 'B';
} else if (marks >= 40) {
    grade = 'C';
} else {
    grade = 'F';
}
~~~

Conditions are checked in order. The first true branch runs; remaining branches are skipped. Order thresholds from most restrictive to least restrictive when appropriate.

### Dangling else

An else belongs to the nearest unmatched if.

~~~c
if (a > 0)
    if (b > 0)
        puts("both positive");
    else
        puts("a was positive, b was not");
~~~

Braces eliminate ambiguity and prevent many maintenance bugs:

~~~c
if (a > 0) {
    if (b > 0) {
        puts("both positive");
    } else {
        puts("a was positive, b was not");
    }
}
~~~

---

## 2.3 switch selection

switch selects among integer-like constant cases. A break normally prevents execution from falling through to the next case.

~~~c
switch (choice) {
    case 1:
        puts("Add");
        break;
    case 2:
        puts("Subtract");
        break;
    case 0:
        puts("Exit");
        break;
    default:
        puts("Invalid choice");
}
~~~

Without break, execution continues into the following case. Intentional fall-through should be clearly documented and may be annotated for the compiler.

C switch does not directly select on strings or floating-point values. Use if/else or a different data-driven method where needed.

---

## 2.4 for loop

A for loop has initialization, condition, and update expressions:

~~~text
for (initialization; condition; update) {
    statements;
}
~~~

Execution order:

1. Initialization runs once.
2. Condition is checked.
3. If true, body runs.
4. Update runs.
5. Return to step 2.
6. If condition is false, exit.

**Example: sum 1 through n**

~~~c
int total = 0;
for (int i = 1; i <= n; i++) {
    total += i;
}
~~~

The condition i <= n includes n. The loop handles n=0 by executing zero times if n is non-negative.

---

## 2.5 while and do-while loops

A while loop checks before each iteration:

~~~c
int count = 1;
while (count <= 5) {
    printf("%d\n", count);
    count++;
}
~~~

A do-while loop checks after the body, so the body runs at least once:

~~~c
int choice;
do {
    printf("Enter 0 to stop: ");
    if (scanf("%d", &choice) != 1) {
        return 1;
    }
} while (choice != 0);
~~~

Choose the loop based on the problem: while may run zero times; do-while runs at least once.

---

## 2.6 Loop invariants and termination

A **loop invariant** is a statement that remains true before and after each iteration. It helps explain correctness.

For summing values a[0] through a[n−1]:

~~~text
Before iteration i: total equals the sum of a[0] through a[i−1].
After adding a[i]: total equals the sum of a[0] through a[i].
~~~

A loop terminates when its stopping condition eventually becomes false or a deliberate exit occurs.

A **variant** is a quantity that moves toward a bound on every iteration. For i=0; i<n; i++, the quantity n−i decreases toward zero.

### Example invariant: sum of a prefix

At the start of iteration i, total equals the sum of elements with indexes less than i. Initially i=0 and the prefix is empty, so total=0. The loop adds a[i], then advances i. The invariant remains true for the next iteration. When i=n, the accumulated total is the sum of the whole array.

---

## 2.7 break, continue, and nested loops

- **break** exits the nearest enclosing loop or switch.
- **continue** skips the rest of the current loop-body iteration.
- In a nested loop, break exits only the inner or nearest loop.

~~~c
for (int i = 0; i < n; i++) {
    if (a[i] < 0) {
        continue; /* skip negative value */
    }
    total += a[i];
}
~~~

Control transfer should be clear. A helper function can be easier to understand than many nested breaks.

---

## 2.8 Operator precedence and associativity

Precedence determines which operator groups first; associativity determines grouping when precedence is equal. Parentheses should be used for clarity.

~~~c
int result = a + b * c;       /* a + (b*c) */
int same = (a + b) * c;       /* different grouping */
int relation = a < b && b < c;
~~~

Do not confuse mathematical notation with C evaluation order. In a full C expression, precedence does not always determine which operand is evaluated first.

### Side effects and unsequenced modification

Avoid modifying a scalar more than once in a single expression when the evaluations are not properly sequenced.

~~~c
int i = 1;
int x = i++ + i++; /* undefined behavior in C */
~~~

Write separate statements instead:

~~~c
int first = i++;
int second = i++;
int x = first + second;
~~~

This is important for GATE-style output tracing: if behavior is undefined by the C standard, the correct answer is not a guessed compiler output.

---

## 2.9 Short-circuit and conditional operators

Logical operators short-circuit:

~~~c
if (ptr != NULL && *ptr == target) {
    /* safe to dereference after non-null test */
}
~~~

For &&, if the left operand is false, the right operand is not evaluated. For ||, if the left operand is true, the right operand is not evaluated.

The conditional operator chooses between two expressions:

~~~c
int minimum = (a < b) ? a : b;
~~~

The ?: operator is an expression, not a substitute for every multi-step if/else. Use it when the result remains readable.

---

## 2.10 Common loop patterns

### Counter-controlled loop

~~~c
for (int i = 0; i < n; i++) {
    /* exactly n iterations when n >= 0 */
}
~~~

### Sentinel-controlled loop

~~~c
int value;
while (scanf("%d", &value) == 1 && value != -1) {
    total += value;
}
~~~

The sentinel -1 is safe only if -1 cannot be ordinary data under the problem's rules.

### Search with early exit

~~~c
int found = 0;
for (int i = 0; i < n; i++) {
    if (a[i] == target) {
        found = 1;
        break;
    }
}
~~~

### Nested loops

For n rows and m columns:

~~~c
for (int row = 0; row < n; row++) {
    for (int col = 0; col < m; col++) {
        /* process one cell */
    }
}
~~~

The inner body runs n×m times. If n=m, the time is O(n²).

---

## 2.11 Trace tables

A trace table records values in execution order.

### Example: trace a loop

~~~c
int i = 1;
int total = 0;
while (i <= 4) {
    total += i;
    i++;
}
~~~

| Check/iteration | i before body | total before | total after | i after |
|---|---:|---:|---:|---:|
| Start | 1 | 0 | 1 | 2 |
| 2 | 2 | 1 | 3 | 3 |
| 3 | 3 | 3 | 6 | 4 |
| 4 | 4 | 6 | 10 | 5 |
| Final check | 5 | 10 | — | Exit |

A good exam trace includes initial values, each branch decision, and the final output.

### Example: continue effect

~~~c
int sum = 0;
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;
    sum += i;
}
~~~

The values added are 1, 2, 4, and 5. Final sum = 12.

---

## 2.12 Common errors and repairs

| Error | Why it happens | Safer approach |
|---|---|---|
| i <= n when valid indexes are 0..n−1 | Confuses count with last index | Use i < n |
| Missing update in while loop | Condition never changes | Identify a loop variant and update it |
| if (x = 0) | Assignment mistaken for comparison | Use x == 0 |
| Accumulator not initialised | Undefined or indeterminate starting value | Set total/count before the loop |
| Extra semicolon after if/for | Empty controlled statement | Keep braces and inspect formatting |
| Switch fall-through | Missing break | Add break or document intentional fall-through |
| Compare floating value with exact equality | Rounding | Use problem-appropriate tolerance |
| Undefined side-effect expression | Assumed left-to-right evaluation | Split modifications into separate statements |

---

## 2.13 Worked examples

### A. Find factorial iteratively

~~~c
long long factorial(int n) {
    if (n < 0) {
        return -1; /* chosen error convention for this example */
    }
    long long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
~~~

This example's return convention is not ideal for every API because -1 could be confused with a valid result in another problem. A robust interface can return a status separately. Factorial overflows long long quickly, so the permitted range must be specified.

### B. Count positive, negative, and zero values

~~~c
int positive = 0, negative = 0, zero = 0;
for (int i = 0; i < n; i++) {
    if (a[i] > 0) {
        positive++;
    } else if (a[i] < 0) {
        negative++;
    } else {
        zero++;
    }
}
~~~

The three counts sum to n, assuming every input element is processed once.

### C. Find the first divisor

~~~c
int divisor = 2;
while (divisor < n && n % divisor != 0) {
    divisor++;
}
if (divisor == n) {
    puts("No divisor below n");
}
~~~

This is not a complete prime test because n<2 needs separate handling. Specifications and boundary cases must accompany algorithms.

---

## 2.14 Questions and answers

1. **What does a for loop's initialization do?**  
   Runs once before the first condition check.

2. **Which loop always executes its body at least once?**  
   do-while.

3. **What does break exit?**  
   The nearest enclosing loop or switch.

4. **What does continue do?**  
   Skips the remainder of the current iteration and proceeds to the next loop check/update as defined by the loop form.

5. **What is a loop invariant?**  
   A property true before and after each iteration that helps prove correctness.

6. **What is an off-by-one error?**  
   A boundary mistake that makes a loop run one time too many or too few.

7. **Does C guarantee left-to-right evaluation of f()+g()?**  
   No. Operand evaluation order is generally unspecified unless sequencing rules state otherwise.

8. **Why are braces useful for if and loops?**  
   They clearly group statements and prevent accidental control-flow changes.

9. **How many times does a nested n by m loop run its inner body?**  
   n×m times.

10. **What is undefined behavior in an output-tracing question?**  
    The standard gives no required result; a particular compiler output cannot be treated as portable.

### GATE/BPSC practice with solutions

1. Trace: for(i=0,sum=0;i<4;i++) sum += 2*i;  
   **Solution:** add 0, 2, 4, 6. Final sum = 12.

2. How many times does the inner statement run?

~~~c
for (i = 1; i <= 5; i++)
    for (j = 1; j <= i; j++)
        count++;
~~~

**Solution:** 1+2+3+4+5 = 15 times.

3. What is printed?

~~~c
int x = 0;
if (x == 0 || 10 / x > 1)
    puts("yes");
~~~

**Solution:** yes. The left side of || is true, so the division is not evaluated.

4. What does the outer loop do after a break inside the inner loop?  
**Solution:** It continues with its next iteration; break exits only the nearest enclosing loop.

5. Is i = i++ + 1 safe?  
**Solution:** No. It modifies i in an unsequenced way in the expression and has undefined behavior.

### Multiple-choice check

1. A do-while body runs: A. zero or more times B. at least once C. exactly twice D. only if initialized  
2. break inside nested loops exits: A. all loops B. nearest loop C. program always D. function  
3. i < n with i from 0 through n−1 runs: A. n times B. n+1 times C. n−1 times D. once  
4. The operands of + in C are generally evaluated: A. left-to-right B. right-to-left C. in unspecified order D. never  
5. A loop invariant is used to reason about: A. correctness B. file extension C. network delay D. CPU brand

**Answers:** 1-B, 2-B, 3-A, 4-C, 5-A.

### Draw-from-memory task

Draw flowcharts for: (a) sum 1 through n, and (b) repeatedly read values until a sentinel appears. Mark initialization, decision, update, and output.
