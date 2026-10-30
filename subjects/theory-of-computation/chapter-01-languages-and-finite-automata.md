# Chapter 1 — Languages and Finite Automata

## 1. Alphabets, strings, languages
An alphabet Σ is a finite set of symbols. A string is a finite sequence from Σ; ε is the empty string. Σ* is all finite strings, including ε; Σ+ excludes ε. A language is any subset of Σ*.

Operations include union, concatenation, and Kleene star. If L={a,b}, then L²={aa,ab,ba,bb}; L* contains ε and every finite sequence of a/b.

## 2. Deterministic finite automata
A DFA is (Q,Σ,δ,q₀,F): states, alphabet, transition function, start state, accepting states. It reads one symbol per transition and accepts if the final state lies in F.

~~~text
even number of 1s:
(q_even) --1--> (q_odd) --1--> (q_even)
     ^0              ^0
0 loops on both states; start/accept q_even
~~~

Trace 101: q_even→q_odd→q_odd→q_even; accepted. A DFA has exactly one transition for each state-symbol pair (a complete DFA may include a dead state).

## 3. Nondeterminism
An NFA may have zero, one, or many next states; it accepts if at least one path ends accepting. ε-NFA may move without reading input. Nondeterminism does not add language-recognition power: subset construction converts an NFA to a DFA whose states are subsets of NFA states. The DFA can have up to 2^n states.

## 4. Product construction
To recognize intersection or union of DFA languages, use state pairs (p,q), transition both components, and choose accepting pairs appropriately. For intersection both components accept; for union at least one accepts. This constructs regular languages from regular languages.

## 5. Questions and practice
**Q1.** Does Σ* contain ε? **A:** Yes.
**Q2.** When does a DFA accept? **A:** After all input is read, its state is accepting.
**Q3.** Does an NFA accept only if every path accepts? **A:** No, at least one path must accept.
**Q4.** What is the maximum subset-DFA state count from n NFA states? **A:** 2ⁿ.
Practice: design DFA for binary strings ending 01; strings with an even number of a's; trace all prefixes; convert a 2-state NFA by subset construction.

## Notebook work
Draw a DFA for even parity and a transition table for strings ending in 01.


## Worked automaton construction: strings ending in 01

Keep the longest suffix of input that is also a prefix of target 01:
- q0: no useful suffix.
- q1: last symbol is 0.
- q2: suffix 01 seen; accepting.

~~~text
       0           1
q0 --------> q1 -------> q2*
^             |           |
|             |0          |1
+------1------+           +---> q0
                            on 0 -> q1
~~~
Transition table: q0:0→q1,1→q0; q1:0→q1,1→q2; q2:0→q1,1→q0. Trace 1101: q0→q0→q1→q2, accept. Trace 010: q0→q1→q2→q1, reject because final suffix is 0.

For subset construction, an NFA state set is updated by taking the union of transitions from all current states; for ε-NFA, take ε-closure before and after consuming a symbol. The empty subset is a legitimate dead state.

**Exam checks:** DFA transition function is single-valued; an NFA string is accepted if at least one computation accepts; ε is a string of length zero, not a symbol. Practice: build DFA for strings with exactly one 1; draw NFA for substring 101; convert a small NFA to subset states.