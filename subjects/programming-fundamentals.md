# Programming Fundamentals — Notebook 02

> **How to use this note:** Copy the definitions, diagrams, and worked traces into a notebook in your own handwriting. For each code example, first predict the output, then run or trace it, and explain why it works. Examples use readable Python and C where useful; the principles transfer to other languages. This is a broad foundation, not a substitute for the exact language topics named in a current exam notification.

## Learning checklist

By the end of this chapter, you should be able to:

- Turn a problem into an algorithm, pseudocode, and a flowchart.
- Use variables, types, expressions, input/output, conditions, and loops.
- Write and trace functions, arrays/lists, strings, dictionaries, and simple file operations.
- Explain scope, mutability, references, recursion, errors, and basic memory ideas.
- Test and debug a program systematically.
- Solve beginner programming problems and explain your choices clearly in an interview or classroom.

---

## 1. Problem solving and algorithms

A **problem** describes an input, the required result, and any rules or constraints. Before coding, clarify:

1. What input is given?
2. What output is expected?
3. What rules connect the input to the output?
4. What values are valid, and what should happen for invalid or boundary cases?
5. How large can the input be?

An **algorithm** is a finite, ordered, unambiguous set of steps for solving a problem. A good algorithm:

- accepts the required input;
- produces the required result;
- terminates after a finite number of steps;
- gives the right result for valid cases;
- handles constraints and edge cases.

A **program** is an implementation of an algorithm in a programming language.

### Example: average of three marks

~~~text
INPUT mark1, mark2, mark3
total = mark1 + mark2 + mark3
average = total / 3
OUTPUT total, average
~~~

Notice that an algorithm describes the logic without committing to Python or C syntax.

### Problem-solving cycle

~~~mermaid
flowchart TD
    A[Understand requirements] --> B[Identify input, output, constraints]
    B --> C[Design algorithm]
    C --> D[Trace examples and edge cases]
    D --> E[Implement in a language]
    E --> F[Test and debug]
    F --> G[Explain and maintain]
    F --> C
~~~

### Pseudocode conventions

Pseudocode is a readable bridge between a problem statement and code. It is not governed by one universal syntax. Common conventions include:

- INPUT or READ to obtain data.
- OUTPUT or PRINT to display a result.
- SET or assignment to store a value.
- IF / ELSE for decisions.
- FOR or WHILE for repetition.
- FUNCTION for a reusable operation.
- Indentation to show which instructions belong to a block.

---

## 2. Flowcharts

A **flowchart** is a diagram showing the control flow of an algorithm.

| Shape | Common meaning |
|---|---|
| Oval or rounded rectangle | Start or end |
| Rectangle | Processing or assignment |
| Parallelogram | Input or output |
| Diamond | Decision with branches |
| Arrow | Direction of control |
| Circle | Connector in a large flowchart |

### Example: check whether a number is even

~~~mermaid
flowchart TD
    A([Start]) --> B[/Read n/]
    B --> C{n mod 2 equals 0?}
    C -- Yes --> D[/Print Even/]
    C -- No --> E[/Print Odd/]
    D --> F([End])
    E --> F
~~~

Use a decision diamond for a condition. Label its outgoing arrows, such as Yes/No or True/False. Make sure every path eventually reaches an appropriate end or loop.

---

## 3. Programs, source code, and language concepts

- **Source code:** human-readable instructions written in a programming language.
- **Syntax:** rules for forming valid statements.
- **Semantics:** meaning of valid statements.
- **Runtime:** period when a program is executing.
- **Library:** reusable code supplied by a language or package.
- **API:** an agreed interface through which code uses another component or service.
- **Program state:** values held by variables and other storage at a particular point.
- **Deterministic behavior:** the same inputs and state produce the same result under the same conditions.

A program may be syntactically correct but still have the wrong logic. It may also depend on external conditions such as files, network services, current time, or random values.

### Python and C at a glance

| Concept | Python | C |
|---|---|---|
| Type checking | Dynamically typed names; objects have types | Statically typed declarations |
| Blocks | Indentation | Braces |
| Variable declaration | Assignment creates/binds a name | Declare a type before use |
| Common text value | str object | char array/string convention |
| Collection | Built-in list, tuple, dict, set | Arrays and structs; libraries add more |
| Memory management | Mostly automatic reference counting and garbage collection | Programmer manages many allocations explicitly |
| Errors | Exceptions are built in | Return codes and other conventions; no built-in exception mechanism |
| Typical learning focus | Concise, readable application code | Types, memory, low-level representation |

Languages and implementations have nuances. Python implementations may compile source to bytecode, and C toolchains have multiple translation stages.

---

## 4. Variables, constants, and identifiers

A **variable** is a named place or binding used to refer to a value that can change during computation.

~~~python
age = 20
age = age + 1
print(age)  # 21
~~~

A **constant** is a value intended not to change. Some languages enforce constants; Python commonly signals the convention with an uppercase name:

~~~python
MAX_ATTEMPTS = 3
~~~

### Identifier rules

An **identifier** is a name for a variable, function, class, or other program element.

Common rules (check the language):

- Use letters, digits, and underscores where allowed.
- Do not begin with a digit in Python or C.
- Do not use reserved words such as if or while as names.
- Names are case-sensitive in Python and C: count and Count are different.
- Prefer meaningful names such as total_marks over vague names such as x, except for short loop indexes.

In C, a declaration specifies the type:

~~~c
int age = 20;
double average = 81.5;
char grade = 'A';
~~~

### Assignment is not the same as equality

- **Assignment** stores or binds a value: Python uses =; C uses =.
- **Equality comparison** asks whether values are equal: Python and C use ==.
- In mathematics, = commonly states equality. In code, the language gives these symbols distinct roles.

~~~python
score = 10        # assignment
is_pass = score >= 5  # comparison result: True
~~~

---

## 5. Data types and conversion

A **data type** describes the kind of value and the operations that are meaningful for it.

| Type family | Examples | Use |
|---|---|---|
| Integer | 7, -12 | Whole numbers |
| Floating point | 3.14, -0.5 | Approximate real-number calculations |
| Boolean | True, False | Conditions |
| Character | 'A' in C | A single character value |
| String/text | "Bihar", "hello" | Text |
| Array/list | [4, 8, 12] | Ordered collection |
| Tuple/record/struct | Point(2, 5) | Group related fields |
| Map/dictionary | {"name": "Asha"} | Key-to-value lookup |
| Set | {2, 4, 6} | Unique values |
| Null-like value | None, NULL | Absence of a value, with language-specific rules |

### Type conversion

**Conversion** changes or interprets a value as another type.

~~~python
text = "42"
number = int(text)
price = float("19.50")
label = str(number)
~~~

A conversion can fail if the input is invalid, such as int("forty"). Decide how the program should handle bad input.

In C, conversions can be implicit or explicit. Narrowing a value can lose information:

~~~c
double amount = 9.8;
int whole = (int) amount;  /* whole becomes 9; fractional part is discarded */
~~~

**Integer overflow:** Fixed-width integer types in C have limited ranges. Unsigned arithmetic wraps modulo a power of two; signed overflow in C is not a safe way to obtain a wrapped answer and can have undefined behavior. Python integers grow to arbitrary precision subject to available memory. Floating-point numbers are approximate in both languages.

---

## 6. Input and output

A program receives input, processes it, and presents output.

~~~python
name = input("Name: ")
age = int(input("Age: "))
print("Hello,", name)
print("Next year:", age + 1)
~~~

The input() function in Python returns text. Convert it when numeric operations are required.

In C, scanf can read formatted input, but it must be used carefully. For example, %d expects a pointer to int:

~~~c
#include <stdio.h>

int main(void) {
    int age;
    if (scanf("%d", &age) != 1) {
        printf("Invalid input\n");
        return 1;
    }
    printf("Next year: %d\n", age + 1);
    return 0;
}
~~~

For robust text input in C, fgets is often preferable to an unchecked or unbounded string read. Always check whether input succeeded.

**Formatting:** A formatted output statement combines text and values. Python f-strings and C printf are common examples.

~~~python
name = "Asha"
marks = 84
print(f"{name} scored {marks} marks")
~~~

---

## 7. Operators and expressions

An **operator** performs an operation. An **operand** is the value the operator acts on. An **expression** combines values, variables, and operators to produce a result.

### Main operator groups

| Group | Examples | Meaning |
|---|---|---|
| Arithmetic | +, -, *, /, %, ** in Python | Calculation |
| Comparison | ==, !=, <, <=, >, >= | Produces a Boolean result |
| Logical | and, or, not in Python; &&, \|\|, ! in C | Combines conditions |
| Assignment | =, +=, -= | Stores or updates a value |
| Membership | in, not in in Python | Tests membership in a collection |
| Identity | is, is not in Python | Tests whether names refer to the same object |
| Bitwise | &, \|, ^, ~, <<, >> | Operates on integer bits |

### Common language differences

- Python ** raises a value to a power; C uses library functions such as pow for general powers.
- Python // performs floor division; C integer division truncates toward zero.
- The remainder operator is % in both Python and C, but negative operands can produce language-specific results.
- Python uses and, or, not; C uses &&, ||, !.
- In C, a condition can use a numeric value as true/false; Python has explicit Boolean values plus truth-value rules.
- In Python, == compares values in many cases, while is checks object identity. Use is None for the None check, not as a general replacement for ==.

### Precedence and parentheses

Operators have precedence rules. When an expression is not obvious, add parentheses.

~~~python
result = (a + b) * c
is_valid = (age >= 18) and (has_id is True)
~~~

Do not rely on remembering every precedence rule in a long expression. Clear parentheses reduce mistakes.

### Short-circuit evaluation

For A and B, many languages do not evaluate B if A already determines the result. For A or B, B may be skipped when A is already true. This is called **short-circuit evaluation**.

~~~python
if divisor != 0 and value / divisor > 2:
    print("Condition is true")
~~~

The left condition protects the division from a zero divisor in Python. Keep side effects out of complex conditions where possible.

---

## 8. Decision making: if, else, and selection

A **conditional** selects which instructions run based on a Boolean condition.

~~~python
marks = 67

if marks >= 80:
    grade = "A"
elif marks >= 60:
    grade = "B"
elif marks >= 40:
    grade = "C"
else:
    grade = "Needs improvement"

print(grade)
~~~

Conditions are checked in order; the first true branch runs. Put more specific or higher-threshold cases first when that is the intended rule.

### C equivalent

~~~c
if (marks >= 80) {
    grade = 'A';
} else if (marks >= 60) {
    grade = 'B';
} else {
    grade = 'C';
}
~~~

### Nested conditions and guard clauses

A condition may appear inside another condition, but too much nesting is hard to read. A **guard clause** handles an invalid or exceptional case early:

~~~python
def divide(a, b):
    if b == 0:
        return None
    return a / b
~~~

Other selection constructs include switch in C and match in some languages. Use them when choosing among a set of cases makes the program clearer.

---

## 9. Repetition: loops

A **loop** repeats a block of instructions. Each repetition is an **iteration**.

### For loop

Use a for loop when iterating over a known sequence or range.

~~~python
for number in range(1, 6):
    print(number)
~~~

Python range(1, 6) produces 1, 2, 3, 4, 5; the stop value 6 is excluded.

### While loop

Use a while loop when repetition continues as long as a condition remains true.

~~~python
count = 1
while count <= 5:
    print(count)
    count += 1
~~~

A loop must make progress toward its stopping condition. If the condition never becomes false and no break occurs, the loop is infinite.

### C for loop

~~~c
for (int count = 1; count <= 5; count++) {
    printf("%d\n", count);
}
~~~

The three parts are initialization, continuation condition, and update.

### Loop control

- **break:** exits the nearest loop.
- **continue:** skips the remaining statements in the current iteration and starts the next one.
- **Sentinel value:** a special input value used to signal that input is finished, such as -1 when ordinary values are non-negative.

Avoid using break and continue so heavily that the main loop logic becomes difficult to follow.

### Off-by-one errors

An **off-by-one error** runs one time too many or too few. Carefully check whether the end value is inclusive or exclusive.

~~~python
# Sum integers 1 through n, inclusive
total = 0
for value in range(1, n + 1):
    total += value
~~~

---

## 10. Functions, parameters, return values, and scope

A **function** is a named, reusable block that performs a task. It can accept **parameters** and return a result.

~~~python
def rectangle_area(width, height):
    return width * height

area = rectangle_area(4, 7)
print(area)  # 28
~~~

- **Parameter:** name in a function definition.
- **Argument:** value supplied when calling the function.
- **Return value:** result sent back to the caller.
- **Procedure:** a term often used for a function that performs an action without returning a meaningful value. Terminology varies by language.
- A function should ideally have one clear responsibility.

### Local and global scope

**Scope** is the region of code where a name can be accessed.

~~~python
def add_tax(price, rate):
    tax = price * rate  # local name
    return price + tax

total = add_tax(100, 0.05)
~~~

The local name tax is created during the call and is not directly available outside the function. Global variables exist outside functions, but excessive global state makes programs harder to test and reason about. Pass needed values as arguments and return results.

### Return versus print

- **return** gives a value to the caller, allowing further computation.
- **print** displays text to a user or console.
- A function that only prints its answer may be harder to reuse than one that returns the answer.

~~~python
def square(n):
    return n * n

answer = square(5)
print(answer)
~~~

### Decomposition

Break a large problem into smaller functions with clear inputs and outputs. This is called **decomposition**. It improves readability, testing, reuse, and team work.

---

## 11. Arrays, lists, tuples, maps, sets, and records

A **collection** stores multiple values.

### Arrays and lists

- An **array** stores an indexed sequence. In C, a basic array has a fixed length and elements of the same declared type.
- A Python **list** is an ordered, mutable sequence that can grow and can hold references to values of different types.
- Indexing commonly starts at 0.

~~~python
scores = [72, 81, 66]
print(scores[0])   # 72
scores.append(90)
scores[1] = 85
~~~

For a collection of length n, valid zero-based indexes are 0 through n - 1. Accessing an invalid index is an error in Python and can cause undefined behavior in C.

### Traversing a collection

~~~python
scores = [72, 81, 66]
total = 0

for score in scores:
    total += score

average = total / len(scores)
~~~

### Tuples and records

A tuple is an ordered collection that is often used for a fixed group of values. A **record** or C **struct** groups named fields that may have different types.

~~~python
student = {"name": "Asha", "marks": 84}
print(student["name"])
~~~

### Dictionaries/maps

A dictionary maps keys to values. Keys are unique within a dictionary.

~~~python
frequency = {}
for letter in "banana":
    frequency[letter] = frequency.get(letter, 0) + 1
~~~

Hash-based dictionaries typically provide average O(1) lookup, insertion, and deletion, but exact performance depends on implementation and inputs.

### Sets

A set stores unique values and supports membership and set operations.

~~~python
unique_marks = {72, 81, 72}
print(unique_marks)  # contains 72 and 81
~~~

Do not assume a set has a meaningful sorted order.

---

## 12. Strings and text processing

A **string** is a sequence of text characters. Text details can vary across languages and encodings.

- Python strings are immutable: an operation that appears to modify a string creates a new string.
- C commonly represents strings as arrays of char ending with a null character, written as '\0'.
- Character counts may not equal byte counts in Unicode text. Some visible symbols are made from multiple code points.

~~~python
text = "Computer Science"
print(len(text))
print(text.lower())
print(text.startswith("Comp"))
words = text.split()
joined = "-".join(words)
~~~

Common tasks include searching, splitting, trimming whitespace, changing case, counting, and validating. Define what counts as a character or word before processing multilingual text.

**Palindrome example:** A palindrome reads the same forward and backward after applying the chosen normalisation rule.

~~~python
def is_palindrome(text):
    cleaned = "".join(ch.lower() for ch in text if ch.isalnum())
    return cleaned == cleaned[::-1]

print(is_palindrome("Never odd or even"))  # True
~~~

---

## 13. Mutability, references, and memory basics

A **mutable** object can be changed after creation. An **immutable** value cannot be changed in place.

- Python lists and dictionaries are mutable; integers, strings, and tuples are immutable.
- In C, an array's elements can be modified if the array is writable.
- A **reference** identifies or provides access to an object. Exact semantics differ across languages.
- A **pointer** in C stores an address. A pointer may be null, invalid, or point to an object; using invalid pointers is unsafe.
- **Aliasing** occurs when two names or references can access the same mutable object.

~~~python
first = [1, 2]
second = first
second.append(3)
print(first)  # [1, 2, 3], both names refer to the same list
~~~

To make a shallow copy in Python, use first.copy() or first[:]. A shallow copy does not recursively copy nested mutable objects.

### C pointers, briefly

~~~c
int value = 10;
int *pointer = &value;  /* pointer stores the address of value */
*pointer = 12;          /* dereference: update value through pointer */
~~~

- &value means the address of value.
- *pointer means the value at the address stored in pointer.
- Always initialise pointers before using them. Check for NULL when a pointer may be absent.
- C arrays do not automatically remember their length. Pass a length alongside an array when needed.

### Memory lifetime

- **Automatic/local storage:** commonly exists for the duration of a function call.
- **Static storage:** exists for the program's lifetime.
- **Dynamic allocation:** requested at runtime. In C, malloc/calloc allocation must eventually be released with free.
- A **memory leak** occurs when dynamically allocated memory is no longer needed but is not released.
- A **dangling pointer** refers to memory that is no longer valid.
- Higher-level languages manage much memory automatically, but references that are unintentionally retained can still cause memory use to grow.

Do not confuse a language-level variable with a guaranteed physical memory layout. Compilers and runtimes may optimise storage.

---

## 14. Recursion

**Recursion** occurs when a function calls itself, directly or indirectly. A recursive solution needs:

1. A **base case** that stops recursion.
2. A **recursive case** that reduces the problem toward the base case.

~~~python
def factorial(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    if n == 0:
        return 1
    return n * factorial(n - 1)
~~~

Trace factorial(4):

~~~text
factorial(4) = 4 × factorial(3)
             = 4 × 3 × factorial(2)
             = 4 × 3 × 2 × factorial(1)
             = 4 × 3 × 2 × 1 × factorial(0)
             = 4 × 3 × 2 × 1 × 1
             = 24
~~~

Each call uses stack space. Deep recursion can exceed the runtime's recursion limit or stack capacity. An iterative loop is often more memory-efficient for simple repetition.

---

## 15. Errors, exceptions, debugging, and testing

### Three common error categories

- **Syntax/compile-time error:** code violates language rules, such as a missing colon in Python or a missing semicolon in C.
- **Runtime error/exception:** a problem occurs while running, such as division by zero, invalid input conversion, or a missing file.
- **Logical error:** the program runs but produces the wrong answer.

### Exception handling in Python

~~~python
try:
    age = int(input("Age: "))
except ValueError:
    print("Please enter a whole number.")
else:
    print("Next year:", age + 1)
~~~

Catch an exception you can handle. Avoid hiding every error with a broad catch that silently continues.

C does not have built-in try/catch exceptions. C programs commonly check return values, error codes, and library-specific error indicators.

### Debugging process

1. Reproduce the problem with a small input.
2. Read the error message and identify the failing line.
3. State the expected result and actual result.
4. Trace variables before and after important statements.
5. Reduce the case while preserving the error.
6. Change one likely cause at a time.
7. Run the relevant cases again and record what fixed it.

### Testing basics

- **Test case:** input, execution conditions, and expected output.
- **Normal case:** typical valid input.
- **Boundary case:** edge of an allowed range, such as zero items or the largest permitted value.
- **Invalid case:** input outside the rules.
- **Unit test:** checks a small component such as one function.
- **Integration test:** checks components working together.
- **Regression test:** checks that a previously fixed issue stays fixed.
- **Assertion:** a check that a condition expected by the program is true.

A function that calculates the average should be tested with one value, multiple values, zero values if allowed, negative values if allowed, and invalid data according to its requirements.

---

## 16. File handling

Files allow programs to persist information between runs. File behavior depends on path, permissions, encoding, and mode.

~~~python
# Read UTF-8 text safely and close the file automatically.
with open("notes.txt", "r", encoding="utf-8") as file:
    text = file.read()

# Write text, replacing the previous contents.
with open("summary.txt", "w", encoding="utf-8") as file:
    file.write("Revision complete\n")
~~~

Common modes include:

- r: read an existing file.
- w: write, replacing existing contents or creating a file.
- a: append to the end.
- Binary modes use b, such as rb or wb.

The with statement closes the Python file even if an error occurs. In C, use fopen, check for failure, read/write carefully, and call fclose.

Never assume a relative path points to the same place on every machine. Treat file paths and external data as untrusted input when security matters.

---

## 17. Algorithm tracing and basic efficiency

A **trace table** records variable values after each important step. It helps find logical errors and is useful in school examinations.

### Trace: count positive values

~~~python
values = [-2, 4, 0, 7]
count = 0

for value in values:
    if value > 0:
        count += 1
~~~

| Iteration | value | value > 0? | count after iteration |
|---:|---:|---|---:|
| Start | — | — | 0 |
| 1 | -2 | No | 0 |
| 2 | 4 | Yes | 1 |
| 3 | 0 | No | 1 |
| 4 | 7 | Yes | 2 |

### Basic time complexity

**Time complexity** describes how an algorithm's work grows with input size n. **Space complexity** describes how additional memory use grows. Big O notation expresses an upper-growth class while ignoring constant factors and smaller terms.

| Complexity | Example pattern |
|---|---|
| O(1) | Access one array element by index |
| O(log n) | Repeatedly halve a sorted search range |
| O(n) | Scan every item once |
| O(n log n) | Common efficient comparison sorts |
| O(n²) | Compare every pair using two nested loops |

Examples:

~~~python
# One pass: O(n) time, O(1) extra space
total = 0
for value in values:
    total += value

# Every pair: O(n^2) time
for i in range(len(values)):
    for j in range(i + 1, len(values)):
        print(values[i], values[j])
~~~

Complexity is a model, not a stopwatch. Constant factors, memory, input distribution, and hardware also affect practical speed. Detailed searching and sorting belong to the Data Structures and Algorithms module.

---

## 18. Worked programming examples

### Example A: even or odd

~~~python
number = int(input("Enter an integer: "))

if number % 2 == 0:
    print("Even")
else:
    print("Odd")
~~~

The remainder after division by 2 is zero for even integers.

### Example B: sum from 1 through n

~~~python
n = int(input("Enter a non-negative integer: "))
total = 0

for value in range(1, n + 1):
    total += value

print(total)
~~~

For n = 4, total progresses as 0 → 1 → 3 → 6 → 10.

### Example C: largest of three values

~~~python
def largest_of_three(a, b, c):
    largest = a
    if b > largest:
        largest = b
    if c > largest:
        largest = c
    return largest
~~~

This works when values may be equal because the requirement is to return the maximum value, not a unique position.

### Example D: count vowels

~~~python
def count_vowels(text):
    vowels = set("aeiou")
    count = 0
    for character in text.lower():
        if character in vowels:
            count += 1
    return count

print(count_vowels("Computer Science"))  # 6
~~~

This example counts the five basic English vowel letters. Rules for other languages and accented characters need to be defined separately.

### Example E: prime-number check

A prime number is an integer greater than 1 with exactly two positive divisors: 1 and itself.

~~~python
def is_prime(n):
    if n < 2:
        return False
    divisor = 2
    while divisor * divisor <= n:
        if n % divisor == 0:
            return False
        divisor += 1
    return True
~~~

It is enough to test divisors up to the square root of n. If n has a factor larger than its square root, its paired factor is smaller.

### Example F: linear search

~~~python
def linear_search(items, target):
    for index, item in enumerate(items):
        if item == target:
            return index
    return -1
~~~

The algorithm examines items in order. It takes O(n) time in the worst case and O(1) additional space. The return value -1 means not found; choose a return convention that cannot be confused with a valid index.

### Example G: input validation with a function

~~~python
def read_percentage(prompt):
    while True:
        try:
            value = float(input(prompt))
        except ValueError:
            print("Enter a number.")
            continue

        if 0 <= value <= 100:
            return value

        print("Percentage must be between 0 and 100.")
~~~

A loop repeats until input is valid. A function keeps this responsibility separate from the rest of the program.

---

## 19. Short-answer questions with answers

1. **What is an algorithm?**  
   A finite, ordered, unambiguous set of steps for solving a problem.

2. **How do syntax and semantics differ?**  
   Syntax is the form required by a language; semantics is the meaning of valid code.

3. **What is a variable?**  
   A name or binding used to refer to a value in a program.

4. **What is the difference between a parameter and an argument?**  
   A parameter appears in a function definition; an argument is the value passed during a call.

5. **What does a return statement do?**  
   It sends a result to the caller and usually ends the current function call.

6. **How does return differ from print?**  
   Return gives a value to the caller; print displays output.

7. **What is a Boolean expression?**  
   An expression that evaluates to true or false.

8. **What is an iteration?**  
   One execution of a loop body.

9. **What causes an infinite loop?**  
   The loop's stopping condition never becomes false and no exit occurs.

10. **What is an off-by-one error?**  
    A boundary mistake causing one too many or one too few iterations or positions.

11. **What is scope?**  
    The region of a program where a name can be accessed.

12. **What is recursion?**  
    A technique where a function calls itself to solve a smaller version of a problem.

13. **What two parts must a recursive solution have?**  
    A base case and a recursive case that moves toward it.

14. **How does a list differ from a tuple in common Python use?**  
    Lists are mutable; tuples are commonly used as immutable ordered collections.

15. **What does a dictionary store?**  
    Key-to-value associations.

16. **What is a pointer in C?**  
    A variable that stores a memory address.

17. **Name three common error types.**  
    Syntax/compile-time, runtime, and logical errors.

18. **What does a unit test check?**  
    A small unit such as an individual function or component.

19. **What does O(n) describe?**  
    Work that grows approximately in proportion to input size n.

20. **Why should programs validate input?**  
    To reject or handle values that violate assumptions and prevent incorrect or unsafe behavior.

---

## 20. Multiple-choice practice

Choose one answer, then check the key.

1. Which structure repeats code while a condition is true?  
   A. while loop  B. dictionary  C. comment  D. return value

2. Which operator compares values for equality in Python?  
   A. =  B. ==  C. :=  D. **

3. What does a function parameter represent?  
   A. A name in the function definition for an input value  
   B. A file path only  
   C. A syntax error  
   D. A loop exit

4. What is the first valid index in a Python list?  
   A. -1  B. 0  C. 1  D. It depends on the list contents

5. Which is needed to ensure a recursive function eventually stops?  
   A. Base case  B. Global variable  C. Printer  D. Infinite loop

6. Which value is Boolean in Python?  
   A. 0.5  B. "True"  C. True  D. "yes"

7. What does break do inside a loop?  
   A. Exits the nearest loop  B. Skips one output only  C. Defines a function  D. Restarts the computer

8. Which Python type is mutable?  
   A. int  B. str  C. list  D. tuple

9. What is a logical error?  
   A. Program runs but produces the wrong result  
   B. File is stored on disk  
   C. Correct syntax and correct result  
   D. A comment in code

10. Which operation is normally O(n) for a list without extra indexing information?  
    A. Scan to find an arbitrary value  B. Read the first element  C. Assign a local integer  D. Return from a function

11. In C, what does &value commonly mean in an expression?  
    A. Address of value  B. Multiply value by itself  C. Logical NOT  D. End of file

12. Which construct is best for handling a Python conversion failure?  
    A. try/except  B. for/else only  C. import  D. break

13. What does a dictionary associate?  
    A. Keys with values  B. Only adjacent memory addresses  C. Files with monitors  D. Operators with comments

14. What does the range(1, 5) call produce in Python?  
    A. 1, 2, 3, 4  B. 1, 2, 3, 4, 5  C. 0, 1, 2, 3, 4  D. 5 only

15. Which statement best describes a return value?  
    A. A result passed from a function to its caller  
    B. Text that must be printed  
    C. A loop condition  
    D. A variable declaration in C only

16. Which is generally an invalid C pointer use?  
    A. Dereferencing an uninitialised pointer  
    B. Taking the address of a valid variable  
    C. Comparing a pointer with NULL  
    D. Passing a valid pointer to a function

17. What does O(1) mean in complexity analysis?  
    A. Work is bounded independently of input size in the model  
    B. Work grows linearly  
    C. Work grows quadratically  
    D. The program has one line

18. Why should a test include boundary cases?  
    A. Errors often occur at limits such as zero or maximum values  
    B. They make all programs faster  
    C. They replace requirements  
    D. They prove a program has no bugs

19. What does continue do in a loop?  
    A. Skips to the next iteration  B. Ends the whole program  C. Declares a class  D. Returns a list

20. Which statement about Python strings is correct?  
    A. They are immutable  B. They are always ASCII  C. They cannot contain spaces  D. They must end in a null character

### Answer key with brief reasons

1. A — while repeats while its condition is true.  
2. B — == compares equality; = assigns.  
3. A — the parameter receives an argument during a call.  
4. B — Python sequences are zero-indexed.  
5. A — the base case stops recursive calls.  
6. C — True is a Boolean value; "True" is text.  
7. A — break exits the nearest enclosing loop.  
8. C — a list can be changed in place.  
9. A — logical errors affect correctness while code may still run.  
10. A — an unsorted scan may inspect all n elements.  
11. A — unary & obtains an object's address in this context.  
12. A — try/except handles specified exceptions.  
13. A — dictionary entries associate keys and values.  
14. A — the stop value 5 is excluded.  
15. A — return sends a result to the caller.  
16. A — dereferencing an uninitialised pointer is unsafe and undefined.  
17. A — constant-time work does not grow with n in the model.  
18. A — boundary conditions commonly expose mistakes.  
19. A — continue begins the next iteration of the nearest loop.  
20. A — Python string objects cannot be modified in place.

---

## 21. Teaching, coding, and interview practice

For a written exam, define the concept, show a small example, and add a diagram or trace table when useful. For an interview, clarify assumptions, explain the approach, test edge cases, and discuss time and space.

1. Write an algorithm and flowchart to find the largest of three numbers.
2. Write a program to count even and odd values in a list.
3. Trace a loop that sums all integers from 1 through 5.
4. Explain the difference among a variable, a data type, and a value.
5. Explain local and global scope with a small example.
6. Compare for and while loops. Give one use case for each.
7. Explain recursion and trace factorial(3).
8. Describe the difference between a list, tuple, dictionary, and set.
9. Explain why a Python name can refer to a mutable list and how aliasing can cause surprising changes.
10. **Interview prompt:** Given an integer list, return the second-largest distinct value. State assumptions and test duplicates, negative values, and short lists.
11. **Interview prompt:** A function returns the wrong answer only for empty input. How would you diagnose and fix it?
12. **Teacher prompt:** Explain the difference between an algorithm, a flowchart, pseudocode, and a program to a beginner.

### Model outline: second-largest distinct value

- Clarify whether “second largest” means distinct from the maximum and what to return if fewer than two distinct values exist.
- One-pass method: track the largest and second-largest distinct values while scanning.
- Update the second-largest when a new value is below the largest but above the current second-largest.
- Test lists such as [5, 5, 3], [-4, -1, -2], [7], and an empty list.
- Time is O(n); extra space is O(1).
- Do not treat a duplicate maximum as the second-largest distinct value.

### Model outline: empty-input bug

- Reproduce with the empty list and inspect the specification.
- Check assumptions such as initializing a maximum from the first element.
- Decide the required behavior: error, None-like result, or another explicit result.
- Add a boundary test and implement the chosen behavior.
- Retest normal, one-element, and empty cases.

---

## 22. Flashcards

| Front | Back |
|---|---|
| What is an algorithm? | Finite, ordered, unambiguous steps for solving a problem. |
| Syntax versus semantics? | Syntax is valid form; semantics is meaning. |
| Assignment versus equality in Python? | = assigns; == compares. |
| What does a Boolean expression produce? | True or False. |
| When is a while loop useful? | When repetition depends on a condition. |
| What does break do? | Exits the nearest enclosing loop. |
| Parameter versus argument? | Definition name versus value passed during a call. |
| Return versus print? | Return gives a value to the caller; print displays output. |
| Which is mutable in Python: list or tuple? | List is mutable; tuple is commonly immutable. |
| What does a dictionary store? | Key-value associations. |
| What does recursion need? | A base case and a recursive step. |
| What is aliasing? | Multiple references or names access the same object. |
| What is a memory leak? | Allocated memory that is no longer needed but not released. |
| What does O(n) mean? | Work grows approximately in proportion to input size. |
| Name three error types. | Syntax/compile-time, runtime, and logical. |
| What does unit testing check? | A small component such as a function. |

---

## 23. One-page quick revision

~~~text
ALGORITHM = finite, ordered, unambiguous problem-solving steps
PROGRAM = implementation of an algorithm in a language
VARIABLE = name/binding for a value
TYPE = kind of value and supported operations
= assigns; == compares equality in Python and C
if/elif/else = selection
for/while = repetition
break = exit loop; continue = next iteration
FUNCTION = reusable block; parameters receive arguments; return gives result
SCOPE = where a name is accessible
LIST/ARRAY = ordered indexed collection
DICTIONARY/MAP = key-to-value lookup
SET = unique elements
RECURSION = function calls itself; needs base case and progress
MUTABLE object can change; IMMUTABLE object cannot change in place
SYNTAX error = invalid form; RUNTIME error = failure during execution
LOGICAL error = program runs but result is wrong
TEST normal, boundary, and invalid cases
O(1) constant | O(log n) halving | O(n) one scan | O(n²) nested pair work
DEBUG = reproduce -> inspect -> trace -> isolate -> fix -> retest
~~~

### Self-check before moving to the next subject

- Can I write and trace a condition, a loop, and a function without copying?
- Can I explain why range(1, n + 1) includes n in Python?
- Can I find an off-by-one error using a trace table?
- Can I explain mutable lists, references, and a C pointer at a beginner level?
- Can I test a function with normal, boundary, and invalid cases?
- Can I state the time complexity of a single scan?

**Next module in the sequence:** Data Structures and Algorithms — arrays, linked lists, stacks, queues, trees, graphs, searching, sorting, hashing, and complexity practice.
