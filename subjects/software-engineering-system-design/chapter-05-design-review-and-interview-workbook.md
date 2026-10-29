# Chapter 5 — Design Review and Interview Workbook

## 1. Review checklist
For each design, verify:
- Requirements and exclusions are explicit.
- API, data model, ownership, and invariants are clear.
- Normal and failure flows are shown.
- Security, privacy, and authorization are addressed.
- Scale estimates connect to actual choices.
- Monitoring, rollout, and recovery are considered.
- Alternatives and trade-offs are stated.

## 2. Case: request rate limiter
Clarify per-user or per-IP, window semantics, burst allowance, distributed deployment, and failure mode. Fixed window is simple but permits boundary bursts. Sliding log is accurate but stores many events. Token bucket allows controlled bursts with refill rate. Distributed counters need atomic updates and may trade exactness for availability.

## 3. Case: file upload
Use authenticated upload intent, size/type validation, object storage, checksum, asynchronous scan/processing, and status endpoint. Do not trust client MIME type. Make completion idempotent; handle expired upload and partial failure.

## 4. Questions
**Q1.** Why estimate capacity before choosing architecture? **A:** It identifies whether a simple design meets constraints.
**Q2.** What is a single point of failure? **A:** Component whose failure makes required service unavailable.
**Q3.** What would you monitor? **A:** User-visible latency/errors plus saturation and dependency health.
Practice: present one design in 10 minutes; defend a rejected alternative; add a failure and recovery path.

## Notebook work
Write assumptions at top, then draw one architecture and annotate each trade-off.
