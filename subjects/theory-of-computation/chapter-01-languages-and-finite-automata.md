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

## Full worked lesson: build and trace a DFA

A deterministic finite automaton is a 5-tuple (Q, Σ, δ, q0, F): states, alphabet, transition function, start state, and accepting states. After reading each symbol, exactly one next state is determined. A string is accepted only if the state reached after its final symbol belongs to F. The empty string ε leaves the machine in its start state.

### Example: binary strings ending in 01

We need remember the longest suffix of input that could still become the desired ending:

~~~text
             0             1
        +---------+---------------+
 -> q0  | q1      | q0            |
   q1   | q1      | q2 (accept)   |
 * q2   | q1      | q0            |
~~~

Interpretation: q0 means no useful suffix; q1 means the latest useful suffix is 0; q2 means the string currently ends in 01. q2 is accepting, but another input symbol can leave it. Trace 1101: q0→q0→q1→q0→q1? That would be wrong if the table's columns are read incorrectly; trace carefully with the table: 1:q0, 1:q0, 0:q1, 1:q2, so accepted. The accepting state means “the most recently read two symbols are 01,” not “01 has ever appeared.”

A DFA for “contains 01 somewhere” needs a different meaning for the accepting state: once reached, it remains accepting on both 0 and 1. This illustrates a core design method: write down the information the state must remember, then derive transitions from that meaning.

### NFA and subset construction

An NFA may have zero, one, or several next states for a symbol; it accepts if at least one path ends in an accepting state. For an ε-NFA, ε-closure(q) is every state reachable from q using only ε edges, including q. Subset construction makes a DFA whose states are sets of NFA states:
- Start set = ε-closure({q0}).
- On symbol a, take all a-transitions from the set, then apply ε-closure.
- A set is accepting when it contains at least one NFA accepting state.

The DFA can have up to 2^n states for an n-state NFA, though many subsets may be unreachable. This construction proves NFAs and DFAs recognize exactly the regular languages.

### State-design checklist

For every state, write a short invariant such as “the input seen so far ends in 0” or “the pattern has already occurred.” Then test the invariant against every outgoing symbol. If two different histories need different future behavior, they cannot safely share one state.

### Questions

1. Does a DFA accept a string when it visits an accepting state at an intermediate point? No; acceptance is decided after all input is consumed.
2. What does the accepting state mean for “ends in 01”? The final two symbols are 01.
3. Why does an NFA accept if one path accepts? That is the existential-path definition.
