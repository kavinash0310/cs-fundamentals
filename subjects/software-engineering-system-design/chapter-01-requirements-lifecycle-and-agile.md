# Chapter 1 — Requirements, Lifecycle, and Agile Development

## 1. Requirements
Functional requirements describe behavior; non-functional requirements constrain quality (latency, availability, privacy, accessibility, cost). A requirement should be testable, unambiguous, and tied to a user or business need.

Use cases describe actor-goal interactions. User stories are concise prompts, not complete specifications; acceptance criteria make them verifiable. Ask about constraints, failure cases, permissions, and scale.

## 2. Lifecycle
Typical work cycles through discovery, requirements, design, implementation, testing, release, operation, and learning. Waterfall sequences phases; iterative development revisits them. Agile organizes small increments with feedback; Scrum is one framework, not synonymous with all agile work.

Risk-driven planning addresses uncertain/high-impact assumptions early. A prototype can test feasibility but is not automatically production-ready.

## 3. Estimation and trade-offs
Estimate by decomposing work and exposing uncertainty. Use ranges and assumptions rather than false precision. Scope, time, quality, and staffing interact; adding people late can increase coordination work.

## 4. Worked example
Requirement: “Users can reset a forgotten password.” Clarify identity proof, token expiration, rate limits, email delivery, account enumeration, audit logging, and recovery failure. Acceptance tests cover valid token, expired token, reused token, and unknown address response.

## 5. Questions and practice
**Q1.** Functional vs non-functional requirement? **A:** Behavior vs quality constraint.
**Q2.** What makes acceptance criteria useful? **A:** Observable pass/fail outcomes.
**Q3.** Is Scrum the whole agile method? **A:** No.
Practice: write criteria for file upload; identify ambiguous requirement; plan an increment for a small feature.

## Notebook work
Map one requirement from user goal to API, tests, release, and monitoring.
