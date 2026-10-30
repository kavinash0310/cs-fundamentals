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

## Full worked lesson: regex, minimization, and the pumping proof

A regular expression describes a language using union (|), concatenation, and Kleene star (*). For example, over {0,1}, the expression (0|1)*01 describes all binary strings that end in 01. By contrast, (0|1)*01(0|1)* describes strings containing 01 somewhere.

### Minimize by future behavior

Two states are equivalent when every possible remaining suffix either makes both accepting or both rejecting. Start by partitioning states into accepting and non-accepting groups. Repeatedly split a group when states transition into different groups on some symbol. Stop when no split is possible; merge states in the same final block and remove unreachable states.

For the “ends in 01” DFA from the previous lesson, the accepting state cannot merge with q0 or q1 because ε distinguishes it. q0 and q1 also differ: suffix 1 leads from q1 to acceptance but not from q0. All three states are therefore necessary.

### Pumping lemma: what it proves

If L is regular, there is a pumping length p such that every s∈L with |s|≥p can be written s=xyz with:
1. |xy|≤p,
2. |y|>0,
3. xy^i z∈L for every i≥0.

The lemma is a necessary property of regular languages. It is useful to prove a language is **not** regular; satisfying a chosen pumping test does not prove regularity.

### Worked proof that {a^n b^n | n≥0} is not regular

Assume the language is regular with pumping length p. Choose s=a^p b^p. Because |xy|≤p, both x and y lie entirely among the first p a's. Since |y|>0, y=a^k for some k>0. Pump down with i=0: xz has p-k a's followed by p b's, so the counts differ. Thus xz is not in L, contradicting the pumping lemma. Therefore L is not regular.

The order of proof matters: assume regularity, select a string after p is fixed, consider every allowed split, and show one permitted pump breaks membership. Do not pick a convenient split yourself; the lemma says some split exists, so the proof must handle all legal splits.

### Common proof failures

- Choosing s before the opponent's pumping length is fixed.
- Forgetting that y may include more than one symbol.
- Showing only one split fails instead of all legal splits.
- Claiming a language is regular because it passes one pumping argument.

### Quick questions

Is {0^n1^m} regular? Yes: 0*1*. Does pumping lemma prove a language regular? No. In the proof above, why must y contain only a's? Because xy lies within the first p positions of a^p b^p.
