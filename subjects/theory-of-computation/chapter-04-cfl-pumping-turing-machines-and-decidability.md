# Chapter 4 — CFL Pumping, Turing Machines, and Decidability

## 1. Pumping lemma for CFLs
For every sufficiently long string s in a CFL, s=uvxyz with |vxy|≤p, |vy|>0, and uvⁱxyⁱz remains in the language for every i≥0. Unlike regular pumping, two pieces v and y are pumped together.

Use it to show {aⁿbⁿcⁿ:n≥0} is not context-free: choose a long aᵖbᵖcᵖ; the bounded substring vxy cannot cover all three blocks, so pumping changes only one/two symbol counts, breaking equality. Handle possible placements explicitly.

## 2. Turing machines
A TM has finite control, an unbounded tape, and a read/write head. A transition reads a symbol, writes one, changes state, and moves left/right/stays. It can model general algorithms. A decider halts on every input; a recognizer may loop on nonmembers.

Church-Turing thesis is a foundational claim about what is effectively computable, not a theorem proving a specific machine model.

## 3. Decidability and reductions
A language is decidable if a TM decides membership. A mapping reduction A≤mB transforms x to f(x) such that x∈A iff f(x)∈B. If A is undecidable and A reduces to B, then B is undecidable.

Halting problem is undecidable: assume a decider H(M,x); construct a program that does the opposite of H on its own description, yielding contradiction.

## 4. Complexity preview
P: problems decidable in polynomial time. NP: yes-instances have certificates verifiable in polynomial time (equivalently nondeterministic polynomial time). P vs NP is open. NP-hard means every NP problem reduces to it; NP-complete means both NP-hard and in NP.

Do not confuse decidability with efficiency: a decidable problem may require enormous time.

## 5. Questions and practice
**Q1.** What is the CFL pumping form? **A:** uvⁱxyⁱz with |vy|>0 and |vxy| bounded.
**Q2.** Difference between recognizer and decider? **A:** Decider halts on all inputs.
**Q3.** What does A≤mB and A undecidable imply? **A:** B is undecidable.
**Q4.** Is P=NP known? **A:** No.
Practice: trace a TM for unary increment; outline halting contradiction; use a mapping reduction to transfer undecidability.

## Notebook work
Draw a TM configuration and a reduction arrow with the membership equivalence.


## Decidability and reduction direction

A decider for A always halts. A recognizer for A accepts members but may run forever on nonmembers. A language is recognizable if some TM recognizes it; its complement is decidable exactly when both sides are recognizable.

To show B is undecidable, reduce a known undecidable A to B: compute f(x), ask whether f(x)∈B, and use the answer to decide A. The arrow points from hard problem to target. Reversing this direction does not prove B hard.

Halting contradiction in outline: assume H(M,x) correctly says whether M halts on x. Build D(y): if H(y,y) says halts, loop forever; otherwise halt. Run D on its own encoding; either answer contradicts behavior.

For complexity, certificate verification must be polynomial in input length. A problem can be in NP without a known polynomial-time solver.

**Practice:** Distinguish “there exists a halting witness” from deciding all inputs; write the reduction equivalence; identify whether a proposed reduction proves A≤B or B≤A.

## Full worked lesson: limits of CFLs and computation

### CFL pumping lemma example

For a context-free language there is a pumping length p such that any sufficiently long string s can be written uvxyz, with |vxy|≤p, |vy|>0, and uv^i xy^i z∈L for all i≥0. The same pumped pieces v and y are repeated in tandem. To show L={a^n b^n c^n | n≥0} is not context-free, assume it is and choose a^p b^p c^p. Since vxy has length at most p, it cannot cover all three symbol blocks. Pumping i=0 or i=2 changes at least one of the first two counts, while at least one other block count stays p; the three counts can no longer all agree. This contradicts the lemma.

The CFL pumping lemma differs from the regular pumping lemma: it has five pieces and pumps v and y together. Mixing up the two statements is a common exam error.

### Turing machines and decidability

A Turing machine has a finite control, an unbounded tape divided into cells, a read/write head, and a transition rule. It is a mathematical model of algorithmic computation. A language is decidable if some machine halts on every input and accepts exactly its members. It is recognizable if members are eventually accepted, but on non-members the machine may reject or run forever. Thus every decidable language is recognizable, but not every recognizable language is decidable.

### Reduction proof pattern

To prove problem B is undecidable, reduce a known undecidable problem A to B. Construct a computable mapping f such that x∈A iff f(x)∈B. If B had a decider, run it on f(x); that would decide A, a contradiction. The direction matters: known-hard A must map **to** the proposed target B. A reduction does not prove “B is easier.”

The classic Halting Problem asks whether program M halts on input w. If a total decider H(M,w) existed, construct program D(x) that loops when H(x,x) predicts halt and halts when H predicts loop. Running D on its own description forces a contradiction. The assumption that H is a total correct decider is therefore impossible.

### Exam checklist

- Decider: halts on yes and no instances.
- Recognizer: guaranteed to halt and accept yes instances; may loop on no instances.
- Undecidable does not mean “hard to solve quickly”; it means no algorithm solves every instance correctly and halts.
- For a reduction, state the mapping and prove both directions of membership preservation.

### Questions with answers

1. Can a recognizable language have a non-member that loops forever? Yes.
2. What does an undecidability reduction assume about the target decider? That it is a total correct algorithm.
3. Why must vxy not touch all three blocks in the CFL pumping example? Its length is at most p, while each block has p symbols; across three blocks it would need at least two boundary crossings and more than p length for a substring containing symbols of all three.
