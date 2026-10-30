# Chapter 2 — Regular Expressions, Minimization, and Pumping

## 1. Regular expressions
Regular expressions describe regular languages using union (|), concatenation, and star (*). Precedence is star, concatenation, then union; parentheses override it.

Example: (0|1)*01 means all binary strings ending in 01. Construct an NFA from an expression by fragment rules, then determinize if needed. Conversely, state-elimination can derive a regular expression from an automaton.

## 2. DFA minimization
Two states are equivalent if every possible suffix either leads both to acceptance or both to rejection. Equivalent states can be merged. Partition refinement begins with accepting/non-accepting sets, then repeatedly splits states whose transitions lead to different blocks.

A distinguishability table marks pairs separated by some suffix. Minimal DFA is unique up to renaming states.

## 3. Pumping lemma for regular languages
If L is regular, some pumping length p exists such that every string s∈L with |s|≥p can be written s=xyz with |xy|≤p, |y|>0, and xyⁱz∈L for every i≥0.

Use it to prove non-regularity: assume regular, take a long string whose first p symbols have a controlled pattern, then show every legal y causes some pumped string to leave L. Example L={0ⁿ1ⁿ:n≥0}: choose 0ᵖ1ᵖ; y consists only of zeros, and pumping down creates fewer zeros than ones.

The lemma is a necessary property, not a method to prove regularity. A failed choice of decomposition is not enough; the proof must handle every legal decomposition.

## 4. Closure and decision questions
Regular languages are closed under union, intersection, complement, concatenation, star, and reversal. DFA emptiness is decidable by reachability from the start to an accepting state. Equivalence of DFAs is decidable by checking whether their symmetric-difference product has an accepting reachable state.

## 5. Questions and practice
**Q1.** What does xy⁰z equal? **A:** xz.
**Q2.** Does pumping lemma prove a language regular? **A:** No, it is only necessary for regularity.
**Q3.** Is complement of a regular language regular? **A:** Yes; swap accepting and nonaccepting states in a complete DFA.
**Q4.** Why is 0ⁿ1ⁿ not regular? **A:** Pumping a nonempty early block of zeros changes zeros but not ones.
Practice: minimize a small DFA; build a regex for strings containing 101; prove {ww:w∈{0,1}*} is not regular using a suitable argument.

## Notebook work
Write the pumping lemma with all quantifiers and draw a partition-refinement split.


## A complete pumping proof pattern

Suppose L={0ⁿ1ⁿ:n≥0} were regular and let p be its pumping length. Choose s=0ᵖ1ᵖ. Every legal split s=xyz has |xy|≤p and |y|>0, so y=0ᵏ for some k>0. Pump i=0: xz has p-k zeros and p ones, so xz∉L. This contradicts the lemma.

A valid proof chooses one string but must defeat every legal split. Do not pick a favorable split yourself. Pumping lemma is one tool; closure under intersection with a known regular language can simplify proofs.

DFA minimization example: states are equivalent only if no suffix distinguishes them. If q accepts ε but r rejects ε, they are immediately distinguishable. If both agree on ε, test transitions: if some symbol sends them to already-distinguishable blocks, split them.

**Practice:** Prove a language with an equal-count dependency nonregular; for a given DFA, find unreachable states then minimize; use complement and product construction to decide equivalence of two DFAs.