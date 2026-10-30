# Chapter 3 — Analytical and Spatial Reasoning

## 1. Deduction and induction
Deduction applies rules to reach a conclusion that must follow. Induction generalizes from examples and is uncertain. In logic puzzles, separate given facts from assumptions.

Example: All A are B; no B are C. Therefore no A are C. But “some B are A” does not follow unless existence is given.

## 2. Relations and numerical reasoning
Translate verbal relations to equations/tables. For sequences, test differences, ratios, alternating terms, squares, primes, and interleaving. Confirm a rule against every provided term before projecting.

Analogy questions ask for the same relation, not merely related words. State the relation in a sentence before choosing.

## 3. Spatial aptitude
Track transformations explicitly:
- Translation shifts position without rotation.
- Rotation turns around a center.
- Reflection mirrors across an axis.
- Scaling changes size.
For paper folding/cutting, unfold in reverse order; each fold reflects the cut pattern across the fold line.

~~~text
fold left half over right -> cut near folded edge
unfold once -> reflected cut pair
unfold again -> four symmetric cuts
~~~

## 4. Worked check
If a shape rotates 90° clockwise twice, result is 180° clockwise (same as 180°). A mirror reflection is not generally equivalent to rotation; orientation may reverse.

## 5. Questions and practice
**Q1.** Must a conclusion follow from assumptions in deduction? **A:** Yes.
**Q2.** What does reflection change? **A:** Orientation across an axis.
**Q3.** In paper-cut problems, what operation reverses folding? **A:** Unfold in reverse sequence.
Practice: identify a sequence rule; solve a syllogism; rotate a grid shape; mirror a pattern; unfold two folds.

## Notebook work
Sketch transformations on squared paper and label axes and direction.


## Multi-step quantitative examples

Successive percentages: price 800 rises 20% then falls 20%: 800×1.2×0.8=768, a net 4% decrease. Percentage base is original value for a single change, but sequential changes use updated base.

Work rates: A completes in 6 days (1/6 job/day), B in 3 days (1/3). Together rate=1/2 job/day, so time=2 days, assuming constant independent rates.

Permutation vs combination: choose president and secretary from 5 gives 5×4=20; choose two-person team gives C(5,2)=10.

Data graph: if a line chart uses y-axis starting at 90, visual gap 95→100 may look large; calculate absolute difference and inspect full scale.

**Practice with answers:** 30% of 250=75; ratio 2:3 totals 50 gives 20 and 30; mean of 2,4,9=5; probability of at least one head in two fair tosses=3/4; area circle radius 3=9π.

## Extended lesson: translate reasoning into a model

**Ordering puzzle.** Four talks P,Q,R,S occur once each. P is before R; Q is immediately after P; S is last. Since Q follows P immediately and S is last, possible sequence begins P,Q. R must be after P, and the remaining place before S is R, giving P,Q,R,S. Writing constraints first prevents random trial.

**Syllogism.** “All coders are learners. Some learners are teachers.” It follows that some coders are learners, if the class of coders is nonempty under the exam's convention. It does **not** follow that some coders are teachers: the teachers may be learners outside the coder group. Use a Venn diagram and avoid assuming an intersection not stated.

### Direction and distance

Walk 3 km east, then 4 km north. The displacement is the straight-line distance √(3²+4²)=5 km, toward northeast. Total distance walked is 7 km. Distance traveled and displacement are different quantities.

### Spatial reasoning

For a cube painted on all faces and cut into n×n×n equal cubes:
- 8 corner cubes have 3 painted faces.
- 12(n-2) edge-interior cubes have 2 painted faces.
- 6(n-2)² face-interior cubes have 1 painted face.
- (n-2)³ interior cubes have no painted face.
For n=4: 8 corners, 24 edge-interior, 24 face-interior, 8 unpainted interior; total 64.

A mirror reverses left/right relative to the viewer, not top/bottom. For paper folding, track which holes overlap after unfolding: each fold reflects the visible mark across the crease. Sketch axes and coordinates for each fold.

### Answer discipline

Check every condition after deriving an arrangement. For a “must be true” question, test whether a counterexample can satisfy all premises. If one exists, the statement is not guaranteed even if it is possible.

## 6. Spatial reasoning: move, reflect, assemble

Keep a fixed coordinate frame. A translation moves every point by the same displacement; a rotation turns around a specified center; a reflection reverses orientation across a mirror line; scaling changes size by a factor. Track the object and the frame separately.

For a point (x,y) about the origin:

| Operation | Result |
|---|---|
| Translate by (a,b) | (x+a, y+b) |
| Rotate 90° counterclockwise | (−y, x) |
| Rotate 90° clockwise | (y, −x) |
| Rotate 180° | (−x, −y) |
| Reflect across the x-axis | (x, −y) |
| Reflect across the y-axis | (−x, y) |
| Scale uniformly by k about origin | (kx, ky) |

Example: point (2,1) rotated 90° counterclockwise becomes (−1,2); reflecting that result across the y-axis gives (1,2). The order matters: transformations generally do not commute. A reflection followed by a rotation can differ from the same rotation followed by reflection.

For grid questions, mark one distinctive corner, arrow, or color, perform the transformation, then check the other features. A mirror reverses left/right relative to the mirror line; it does not automatically turn the object upside down.

## 7. Two-dimensional and three-dimensional visualization

In 2D, compare orientation, adjacency, and relative position. In a 3D drawing, identify front, side, and top faces before rotating the cube mentally. A cube rotation moves faces while preserving which faces share an edge; a mirror image reverses handedness.

A cube net is valid when six squares fold to six distinct faces without overlap. Opposite faces in a net cannot become adjacent on the folded cube. To solve a painted-cube question, count exposed faces before cutting:

- A corner small cube has 3 painted faces.
- A non-corner edge cube has 2.
- A face-interior cube has 1.
- An interior cube has 0.

For an n×n×n cut with n≥2, these counts are 8, 12(n−2), 6(n−2)², and (n−2)³ respectively. For n=4, that is 8+24+24+8=64 pieces. For n=2 there are no edge-interior, face-interior, or unpainted interior pieces; the formulas correctly give zero.

## 8. Assemble, group, and complete patterns

Assembly questions ask which pieces can form a target without changing the permitted orientation. Compare boundary edges and unique features first; ignore internal edges after joining. Grouping asks for a shared rule: same number of sides, same reflection symmetry, same shaded fraction, or same operation. State the rule and verify every member; one matching feature may be accidental.

For a visual pattern, test a fixed order: count elements, compare positions, compare orientation, inspect shading, then test size or alternation. Example:

~~~text
Pattern:  ↑   →   ↓   ←   ?
Turn each arrow 90° clockwise. Next: ↑
~~~

Do not infer rotation from a moving position alone. A dot may translate while a shape rotates; track both independently.

## 9. Paper folding and cutting

Record each fold as a reflection across its crease. Unfold in reverse order. If a hole is away from a crease, it creates a mirrored hole at each reflected location; a hole on the crease does not double at that fold. A second fold may create up to four symmetric copies, but coincidences on creases reduce the count.

~~~text
Square, fold right half to left across vertical center:
before:  •       crease       folded half
unfold:  •   |   mirror •
Two folds can create four reflected positions:
             • | •
            ---+---
             • | •
~~~

The sketch is schematic: use coordinates and crease lines for the actual question. For a diagonal fold, reflect across the diagonal, not across horizontal or vertical axes. Paper-cut questions may also ask for the final shape after removing a piece; reverse the fold sequence and copy the cut by reflection at every step.

**Worked fold:** A square is folded once along its vertical center, then once along its horizontal center. A hole is punched away from both creases. Unfolding first across the horizontal crease creates a pair; unfolding across the vertical crease mirrors both, giving four holes. If the punch lies on one crease, the corresponding reflection coincides there, so count distinct positions rather than blindly multiplying by four.

## 10. Analytical reasoning checks

For deduction, ask whether a conclusion must follow for every arrangement satisfying the premises. For induction, identify a pattern supported by the examples but remember that a finite sequence can have multiple possible rules. For analogy, state the relationship as a verb phrase (“is a part of,” “measures,” “causes”) and preserve its direction.

Example: all kernels are programs; no programs are rocks. Therefore no kernel is a rock. But “some programs are kernels” is not guaranteed in formal logic unless existence is specified. For induction, 1, 4, 9, 16 suggests squares and next 25, but other rules could fit the four values; use the simplest consistent rule expected by the question.

**Questions and answers**

1. Point (3,−2) reflected across the x-axis? Answer: (3,2).
2. Point (1,4) rotated 90° clockwise about origin? Answer: (4,−1).
3. Does a pure translation change orientation? Answer: No.
4. A square is folded once and a hole is punched away from the crease. How many holes after unfolding? Answer: Two.
5. A 3×3×3 painted cube is cut into unit cubes. How many have exactly one painted face? Answer: 6(3−2)²=6.
6. Is a mirror reflection equivalent to rotating the object 180° in every case? Answer: No; a reflection reverses handedness.
