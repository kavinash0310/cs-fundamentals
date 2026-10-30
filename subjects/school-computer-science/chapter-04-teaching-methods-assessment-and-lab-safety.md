# Chapter 4 — Teaching Computer Science, Assessment, and Lab Safety

## 1. Lesson design
Start from an observable learning objective: “Learner can trace a loop and explain its output.” Activate prior knowledge, model one example, guide practice, then let students solve independently. Use misconception checks before moving on.

Represent concepts in multiple ways: explanation, trace table, diagram, code, and analogy. An analogy is useful only when its limits are also stated.

## 2. Assessment
Formative assessment checks understanding during learning: exit questions, code tracing, peer explanation. Summative assessment evaluates achievement after a unit. Rubrics make criteria transparent: correctness, reasoning, clarity, testing, and safe practice.

A good programming question specifies input, expected output, constraints, and edge cases. Give partial credit for sound reasoning when appropriate.

## 3. Differentiation and inclusion
Provide scaffolds such as starter code, vocabulary, worked examples, and challenge extensions. Use accessible materials, readable contrast, keyboard-friendly tools, and varied participation. Avoid assuming every student has a device or home internet.

## 4. Lab safety and ethics
Teach account privacy, file ownership, respectful collaboration, source attribution, secure handling of credentials, electrical/cable awareness, ergonomics, and appropriate reporting. Follow school policy and the exact official teaching syllabus for recruitment requirements.

## 5. Questions and practice
**Q1.** Formative assessment purpose? **A:** Adjust instruction while learning is in progress.
**Q2.** Why state an analogy's limits? **A:** Prevent learners from extending it incorrectly.
Practice: write one lesson objective; plan a 40-minute lesson on loops; create a three-question misconception check and a rubric.

## Notebook work
Prepare a board plan: objective, diagram, worked example, student practice, exit ticket.


## Build a lesson objective and check misconception

Topic: binary search. Observable objective: “Given a sorted array and target, learner traces each interval and returns its index or not-found result.” Sequence: quick sort prerequisite check; teacher models a trace; class predicts next midpoint; pairs solve; exit ticket checks duplicate/boundary case.

Misconception check: “Can binary search be used on an unsorted array?” Ask for a counterexample, not only yes/no. Learners should explain that discarding half is safe only when ordering supports the comparison.

Formative evidence changes teaching: if many learners update high=mid incorrectly under inclusive bounds, pause and contrast [lo,hi] with [lo,hi). Do not grade only final answer; inspect reasoning and representation.

**Practice:** Create a 10-minute mini-lesson on a loop; specify prerequisite, board diagram, one guided example, one independent task, and an exit question.

## Extended lesson: teach one concept from objective to evidence

A lesson objective should state observable learning. “Understand loops” is vague. “Given a list of numbers, learners will trace a loop and correctly predict the final count” can be assessed. Align the activity and evidence to the objective.

### 40-minute lesson example: loop tracing

- 0–5 min: show a short program and ask what changes on each iteration.
- 5–12 min: teacher models a trace table aloud, including a common off-by-one error.
- 12–22 min: pairs trace a second example; one learner states the current variable, the other records it.
- 22–32 min: learners write a small loop and test it on normal, empty, and boundary inputs.
- 32–37 min: exit ticket asks for output and one-sentence explanation.
- 37–40 min: summarize invariant and preview the next lesson.

Assessment evidence should match the objective. A multiple-choice output question checks tracing; a coding task checks construction; an explanation checks conceptual reasoning. Formative assessment happens during learning and guides the next teaching move; summative assessment judges achievement at a point in time.

### Inclusive and safe lab practice

Offer more than one way to participate: visual trace table, spoken explanation, and hands-on code. Provide readable font/contrast, keyboard-accessible tools, clear vocabulary, and extra time when appropriate. Pair roles should rotate so one learner does not always control the computer. Do not assume every learner has a device or internet at home.

Teach learners to save work, protect credentials, avoid unknown downloads, and report unsafe content. Keep food and liquids away from equipment, route cables safely, and power off devices before connecting hardware. A teacher should model safe account handling and never display student passwords.

### Pedagogical Q&A

1. Why use a trace table? It makes changing state visible and supports systematic debugging.
2. What is formative assessment? Evidence gathered during learning to adjust teaching and feedback.
3. Why include boundary inputs? They expose errors such as empty-list and off-by-one failures.
4. Why rotate pair-programming roles? It gives each learner practice in both explaining and operating.
5. How can a teacher check whether an objective was met? Collect evidence aligned to the stated performance, such as a solved trace plus explanation.
