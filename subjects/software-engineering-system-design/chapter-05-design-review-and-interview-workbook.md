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


## Token-bucket rate limiter and file-upload walkthrough

Token bucket has capacity B and refill rate r tokens/second. A request of cost c is accepted if tokens≥c, then consumes c. If not, reject or delay. With B=10,r=2/s, an empty bucket needs 3 seconds to accumulate 6 tokens; after a burst of 10, one token returns every 0.5 seconds.

Distributed limiters need atomic shared updates or accept approximation. State key cardinality, eviction, clock handling, and failure policy. “Limit 100/minute” is ambiguous about boundary bursts; define semantics.

Upload workflow: authorize upload, issue short-lived presigned URL, store object, verify checksum/size/type, scan asynchronously, mark ready, notify. Use unique object IDs, idempotent completion, retention cleanup for abandoned uploads, and a quarantine state before serving files.

**Practice:** Design a 1MB file limit; handle duplicate completion; explain what happens if object storage succeeds but DB write fails; propose reconciliation.