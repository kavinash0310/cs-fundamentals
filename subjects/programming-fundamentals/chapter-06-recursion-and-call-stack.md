# Chapter 6 — Recursion, Call Stack, and Recursive Tracing

**Course:** Programming Fundamentals  
**Level:** Foundation to GATE/BPSC bridge  
**Prerequisite:** Chapters 1–5

## What you will learn

You will define recursive functions, prove termination using a base case and a decreasing measure, trace calls and returns, estimate recursion depth and time, and compare recursive and iterative solutions.

---

## 6.1 What recursion means

**Recursion** is a technique in which a function calls itself directly or indirectly to solve a smaller instance of the same problem.

A correct recursive function needs:

1. A **base case** with a direct answer.
2. A **recursive case** that reduces the problem toward a base case.
3. A **progress measure** that cannot decrease forever.

~~~mermaid
flowchart TD
    A[Function receives problem] --> B{Base case?}
    B -- Yes --> C[Return direct answer]
    B -- No --> D[Reduce problem]
    D --> E[Call function on smaller problem]
    E --> F[Use returned result]
    F --> G[Return answer]
~~~

If the base case is missing, unreachable, or does not match the recursive case, calls may continue until the program exhausts stack resources.

---

## 6.2 Call stack and activation records

Each function call has an **activation record** (stack frame) containing information such as parameters, local variables, and where execution should resume. Exact frame layout is implementation-dependent.

~~~mermaid
flowchart TB
    A[Most recent call: fact(1)] --> B[fact(2)]
    B --> C[fact(3)]
    C --> D[main]
~~~

Calls are pushed as recursion goes deeper and return in reverse order. This is last-in, first-out behavior.

### Trace factorial(4)

~~~text
fact(4) waits for 4 × fact(3)
  fact(3) waits for 3 × fact(2)
    fact(2) waits for 2 × fact(1)
      fact(1) waits for 1 × fact(0)
        fact(0) returns 1
      fact(1) returns 1
    fact(2) returns 2
  fact(3) returns 6
fact(4) returns 24
~~~

---

## 6.3 Factorial

Mathematical definition:

~~~text
n! = 1,                 when n = 0
n! = n × (n−1)!,        when n > 0
~~~

C implementation:

~~~c
long long factorial(int n) {
    if (n < 0) {
        return -1; /* illustrative error convention only */
    }
    if (n == 0) {
        return 1;
    }
    return (long long)n * factorial(n - 1);
}
~~~

The multiplication is done while calls return. This function uses O(n) time and O(n) stack space. The result overflows long long for sufficiently large n; define an allowed range or use checked arithmetic.

A safer interface would return a status separately from the result rather than using -1 as an error marker.

---

## 6.4 Recursion trace: print before and after call

~~~c
void show(int n) {
    if (n == 0) {
        return;
    }
    printf("%d", n);
    show(n - 1);
    printf("%d", n);
}
~~~

For show(3):

~~~text
Call show(3): print 3, call show(2)
Call show(2): print 2, call show(1)
Call show(1): print 1, call show(0)
Call show(0): return
Return to show(1): print 1
Return to show(2): print 2
Return to show(3): print 3
Output: 321123
~~~

The statements before the recursive call run while descending. Statements after it run while calls return.

---

## 6.5 Sum of the first n integers

~~~c
long long sum_to_n(int n) {
    if (n <= 0) {
        return 0;
    }
    return n + sum_to_n(n - 1);
}
~~~

For n=4:

~~~text
sum(4) = 4 + sum(3)
       = 4 + 3 + sum(2)
       = 4 + 3 + 2 + sum(1)
       = 4 + 3 + 2 + 1 + sum(0)
       = 10
~~~

The recursion depth and work are O(n). An arithmetic formula or loop uses less stack space.

---

## 6.6 Euclid's greatest common divisor

Euclid's algorithm:

~~~text
gcd(a, 0) = |a|
gcd(a, b) = gcd(b, a mod b), when b != 0
~~~

~~~c
int gcd_nonnegative(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd_nonnegative(b, a % b);
}
~~~

Assume inputs are non-negative and the minimum signed integer absolute-value issue is avoided. The smaller nonzero operand decreases quickly; the time is O(log min(a,b)) for positive inputs.

**Trace gcd(48,18):**

~~~text
gcd(48,18) = gcd(18,12)
            = gcd(12,6)
            = gcd(6,0)
            = 6
~~~

---

## 6.7 Recursive power

For non-negative n:

~~~text
power(x, 0) = 1
power(x, n) = x × power(x, n−1)
~~~

This uses O(n) multiplications and O(n) stack frames.

Exponentiation by squaring improves the number of multiplications:

~~~c
long long power(long long x, unsigned int n) {
    if (n == 0) {
        return 1;
    }
    long long half = power(x, n / 2);
    if (n % 2 == 0) {
        return half * half;
    }
    return x * half * half;
}
~~~

This uses O(log n) calls, but result multiplication can overflow. If only modular powers are required, reduce intermediate products modulo the modulus and still consider multiplication overflow.

---

## 6.8 Fibonacci and repeated work

Definition: F(0)=0, F(1)=1, and F(n)=F(n−1)+F(n−2).

~~~c
long long fib(int n) {
    if (n < 0) {
        return -1;
    }
    if (n < 2) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
~~~

This simple recursive version repeats many calls.

~~~mermaid
flowchart TD
    F5[fib(5)] --> F4[fib(4)]
    F5 --> F3a[fib(3)]
    F4 --> F3b[fib(3)]
    F4 --> F2a[fib(2)]
    F3a --> F2b[fib(2)]
    F3a --> F1a[fib(1)]
~~~

The repeated fib(3), fib(2), and smaller calls make time exponential, approximately O(φⁿ), where φ is the golden ratio. The recursion depth is O(n).

A loop or memoization computes each value once in O(n) time. Recursion is not automatically efficient just because its definition is short.

---

## 6.9 Recursive binary search

Binary search requires a sorted array. Each step keeps only the half that may contain the target.

~~~c
int binary_search_recursive(const int a[], int left, int right, int target) {
    if (left > right) {
        return -1;
    }

    int middle = left + (right - left) / 2;
    if (a[middle] == target) {
        return middle;
    }
    if (target < a[middle]) {
        return binary_search_recursive(a, left, middle - 1, target);
    }
    return binary_search_recursive(a, middle + 1, right, target);
}
~~~

Time is O(log n), stack space is O(log n). Be careful with signed index types and empty arrays. An iterative version can use O(1) extra space.

---

## 6.10 Direct and indirect recursion

- **Direct recursion:** function A calls itself.
- **Indirect/mutual recursion:** A calls B and B eventually calls A.

Both require a reachable base case across the call cycle.

~~~c
int is_even(int n);

int is_odd(int n) {
    if (n == 0) return 0;
    return is_even(n - 1);
}

int is_even(int n) {
    if (n == 0) return 1;
    return is_odd(n - 1);
}
~~~

This example assumes n is non-negative. Negative input needs a stated rule.

---

## 6.11 Tail recursion

A recursive call is **tail-recursive** if it is the final operation performed by the current function.

~~~c
int sum_helper(int n, int total) {
    if (n == 0) {
        return total;
    }
    return sum_helper(n - 1, total + n);
}
~~~

C does not require compilers to transform tail recursion into a loop. Do not assume tail recursion removes stack use unless the implementation and optimization guarantee it. For deep input, an explicit loop is safer.

---

## 6.12 Recursion and correctness proof

A common proof uses induction:

1. **Base case:** prove the function returns the correct result for the smallest input.
2. **Inductive assumption:** assume the recursive function is correct for a smaller input.
3. **Inductive step:** show the current call combines the smaller correct result into the correct current result.
4. **Termination:** show a non-negative measure decreases and eventually reaches the base case.

For factorial, n decreases by one until zero. For binary search, the remaining interval shrinks by about half each call.

---

## 6.13 Common recursion errors

- Missing base case.
- Base case not reachable for some input.
- Recursive call does not make the problem smaller.
- Combining the returned result incorrectly.
- Forgetting that code after the call runs during unwinding.
- Returning a value that has expired or using local storage incorrectly.
- Assuming recursion is faster than iteration.
- Ignoring stack limits or arithmetic overflow.
- Repeating work that memoization could avoid.

---

## 6.14 Questions and answers

1. **What is the base case?**  
   A direct case that returns an answer without another recursive call.

2. **What is a recursive case?**  
   A case that calls the function on a smaller instance and combines the result.

3. **What is an activation record?**  
   Information associated with one function call, such as parameters, locals, and return location.

4. **In what order do recursive calls return?**  
   Last call returns first, following stack order.

5. **What is the output of show(3) above?**  
   321123.

6. **Why is naive recursive Fibonacci slow?**  
   It recomputes the same values many times, producing exponential work.

7. **What does tail-recursive mean?**  
   The recursive call is the final operation of the function.

8. **Does C guarantee tail-call optimization?**  
   No.

9. **What is the recursive Euclid step?**  
   gcd(a,b)=gcd(b,a mod b).

10. **How do we prove recursive termination?**  
    Show that a well-founded progress measure decreases and reaches the base case.

### GATE/BPSC practice with solutions

1. How many calls are made by recursive factorial for n=5, including fact(0)?  
   **Solution:** 6 calls: fact(5), fact(4), fact(3), fact(2), fact(1), fact(0).

2. What is returned by f(3)?

~~~c
int f(int n) {
    if (n <= 1) return 1;
    return n + f(n - 1);
}
~~~

**Solution:** f(1)=1, f(2)=3, f(3)=6.

3. Evaluate the printed sequence:

~~~c
void p(int n) {
    if (n == 0) return;
    printf("%d", n);
    p(n - 1);
}
~~~

**Solution:** p(4) prints 4321.

4. What is the depth of recursive binary search on n sorted elements?  
   **Solution:** O(log n), since the candidate interval is approximately halved each call.

5. Why can recursive factorial overflow even if the base case is correct?  
   **Solution:** The mathematical result grows rapidly and may exceed the return type's range.

6. State the time and extra stack space of recursive sum-to-n.  
   **Solution:** O(n) time and O(n) stack space.

### Multiple-choice check

1. A recursive solution requires: A. base case and progress B. global variable C. two loops D. file stream  
2. Recursive calls return in: A. FIFO order B. LIFO order C. random order D. sorted order  
3. Naive Fibonacci time is: A. O(1) B. O(log n) C. exponential D. O(n)  
4. Binary search recursion depth is: A. O(n²) B. O(log n) C. O(1) for all n D. exponential  
5. C guarantees tail-call optimization: A. yes B. no C. only with static D. only for int  
6. factorial(0) conventionally equals: A. 0 B. 1 C. undefined D. n

**Answers:** 1-A, 2-B, 3-C, 4-B, 5-B, 6-B.

### Draw-from-memory tasks

- Draw the call stack for factorial(4) while descending and while returning.
- Draw the repeated-call tree for fib(5) and circle duplicated subproblems.
- Draw one recursive binary-search interval and show how the next interval shrinks.
