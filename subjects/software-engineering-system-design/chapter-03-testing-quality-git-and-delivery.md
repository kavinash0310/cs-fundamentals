# Chapter 3 — Testing, Quality, Version Control, and Delivery

## 1. Testing levels
Unit tests isolate small behavior; integration tests exercise boundaries; end-to-end tests validate user workflows. Test pyramid is a heuristic: many focused fast tests, fewer broad slow tests. Contract tests check service agreements. Property-based tests check invariants over generated inputs.

Good tests are deterministic, independent, and named for behavior. Cover normal, boundary, invalid, and failure cases.

## 2. Test doubles and debugging
A stub supplies canned responses; a mock verifies interactions; a fake is a lightweight working implementation. Prefer real collaborators when practical; excessive mocking can test wiring rather than behavior.

Debug by reproducing, narrowing the cause, checking assumptions, fixing the underlying defect, and adding a regression test.

## 3. Git and review
A commit is a snapshot in history. Branches allow parallel work; merge integrates histories. Review for correctness, readability, security, tests, and scope. Small changes are easier to inspect. Resolve conflicts by understanding both changes, not by blindly choosing one side.

## 4. CI/CD and operations
Continuous integration builds/tests changes automatically. Delivery automates deployable artifacts; deployment releases them. Use staged rollout, health checks, rollback, and observability. A release is successful only if user-impact signals remain healthy.

## 5. Questions and practice
**Q1.** Which test catches real service interaction? **A:** Integration/contract test depending on boundary.
**Q2.** Why add regression test? **A:** Prevent reintroducing a fixed defect.
**Q3.** What is rollback for? **A:** Restore a known-good release after harmful change.
Practice: propose tests for payment retry; diagnose flaky test causes; review a risky database migration.

## Notebook work
Draw a pipeline from commit through build, test, deploy, health check, and rollback.


## Test plan and delivery example

Feature: failed payment retry. Unit tests cover retry-count policy and backoff; integration tests exercise payment adapter and database transaction; contract tests verify provider response assumptions; end-to-end test covers user-visible status. Test timeout, duplicate callback, invalid signature, and eventual success.

A CI pipeline may run formatting/static checks, unit tests, integration tests, package/build, security scan, then deploy to staging. Production rollout can use canary/gradual traffic. Define a health metric and rollback threshold before release.

A flaky test often depends on wall clock, shared mutable state, random order, or external network. Inject a clock, isolate data, control randomness, and use deterministic fakes where appropriate.

**Practice:** Write test cases for a database migration; explain how to revert schema safely when old and new application versions overlap; review a commit that changes only an API response field.