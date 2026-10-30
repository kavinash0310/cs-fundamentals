# GATE CS Previous-Year Walkthrough — Selected 2023 Questions

These explanations paraphrase four questions from the official GATE 2023 CS paper. The 2027 syllabus has been revised, so check each old question against the current syllabus before using it for scoring.

- [Official GATE 2023 CS question paper](https://gate.iitk.ac.in/GATE2023/doc/papers/2023/CS_GATE2023.pdf)
- [Official GATE 2023 CS answer key](https://gate.iitk.ac.in/GATE2023/doc/papers/2023/ANS/CS_ANS_GATE2023.pdf)
- [Current GATE 2027 CS syllabus](https://gate2027.iitm.ac.in/static/doc/GATE2027_Syllabus/CS_GATE2027_Syllabus.pdf)

## Question 23 — User mode to kernel mode

The question asks which events guarantee a transition from user mode into kernel mode. The official answer is **page fault and system call**.

A system call deliberately transfers control to a protected kernel entry point so the operating system can perform a privileged service. A page fault is an exception raised by memory translation/protection hardware; the processor traps into the kernel so it can handle or reject the access. An ordinary function call stays in user mode. A library call such as malloc may sometimes request more memory from the kernel, but the call itself does not guarantee a system call on every invocation.

**Lesson:** Distinguish a language-level function call from a hardware trap or system-call boundary.

## Question 31 — Symmetry in a triple integral

The integrand is the sum of a term odd in y and a term odd in z; each variable is integrated over a symmetric interval around zero. The y-odd term integrates to zero over its symmetric y bounds. The z-cubed term integrates to zero over its symmetric z bounds. Therefore the whole integral is **0**, matching the official answer.

~~~text
if f(-x) = -f(x), then integral from -a to a of f(x) dx = 0
~~~

Check symmetry before expanding or integrating term by term; it can turn a long calculation into one line.

## Question 32 — Convert a radix representation

Convert 132 in base 4 to decimal:

1×4² + 3×4¹ + 2×4⁰ = 16+12+2 = 30.

Now convert 30 to base 5: 30=1×25+1×5+0, so the representation is **110 in base 5**. This matches the official key.

**Check:** 110₅=1×25+1×5+0=30, so conversion is reversible.

## Question 33 — Pipeline execution time

The three stage delays are 10 ns, 20 ns, and 14 ns. With no hazards and one new instruction each cycle, the clock period is the slowest stage: 20 ns. A k-stage pipeline executing n instructions takes k+n−1 cycles after filling. Here that is 3+100−1=102 cycles.

Total time = 102×20 ns = **2040 ns**, matching the official key. The first result appears after the pipeline fills; subsequent instructions complete one per cycle.

## Self-review method

For every old question, record the official key, your derivation, the topic, and whether that topic remains in the revised syllabus. If the key is an option letter, also record the reasoning; the letter alone will not help on a changed question.