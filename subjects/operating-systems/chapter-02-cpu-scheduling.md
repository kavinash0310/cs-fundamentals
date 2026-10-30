# Chapter 2 — CPU Scheduling

## 1. Goals and metrics
A scheduler selects a ready process. Metrics include CPU utilization, throughput, turnaround (completion-arrival), waiting time, response time (first service-arrival), fairness, and deadline satisfaction. Optimize the metric that the question asks for.

## 2. Algorithms
- FCFS: arrival order; simple but convoy effect can delay short jobs.
- SJF: shortest next CPU burst; minimizes average waiting time when bursts are known, but may starve long jobs.
- SRTF: preemptive SJF; preempts for a shorter remaining burst.
- Priority: highest priority first; aging can prevent starvation.
- Round Robin: each ready process gets time quantum q; small q improves response but increases context-switch overhead.
- Multilevel feedback queue: processes move between queues based on behavior; flexible but policy-heavy.

## 3. Worked schedule
Jobs P1(AT=0,BT=5), P2(AT=1,BT=3), P3(AT=2,BT=1), FCFS:
~~~text
0      5      8  9
|  P1  |  P2  |P3|
~~~
Turnaround: P1=5, P2=7, P3=7. Waiting = turnaround-burst: 0,4,6. Always include arrival time and preemption assumptions.

## 4. Questions and practice
**Q1.** Which metric measures time until first response? **A:** Response time.
**Q2.** Why can Round Robin hurt with tiny quantum? **A:** Excess context-switch overhead.
**Q3.** What is aging? **A:** Gradually raising waiting processes' priority.
Practice: draw FCFS and RR Gantt charts; calculate response/wait/turnaround; compare SJF and SRTF; identify starvation.

## Notebook work
For one process table, draw a Gantt chart and calculate each metric separately.


## Round Robin worked schedule

Three jobs arrive at t=0: P1 burst 5, P2 burst 3, P3 burst 1; quantum q=2, no switch cost.
~~~text
0-2 P1 | 2-4 P2 | 4-5 P3 | 5-7 P1 | 7-8 P2 | 8-9 P1
~~~
Completion: P1=9, P2=8, P3=5. Turnaround: 9,8,5. Waiting=turnaround-burst: 4,5,4. Response: P1=0,P2=2,P3=4.

If q becomes very large, RR approaches FCFS. If q is too small, context switches dominate. Preemptive algorithms may improve response but require saving/restoring execution state.

For SRTF, when a new job arrives, compare its burst to the currently running job's remaining time. At each preemption boundary update remaining times. Waiting time is not the same as turnaround; keep formulas visible:
~~~text
turnaround = completion - arrival
waiting = turnaround - CPU burst
response = first start - arrival
~~~

**Practice:** Recompute the schedule with q=1; compare response and turnaround; explain starvation risk for SJF and a role for aging.

## SJF/SRTF worked comparison

Jobs: P1 arrives 0, burst 7; P2 arrives 2, burst 4; P3 arrives 4, burst 1.

Non-preemptive SJF: at time 0 only P1 is ready, so run P1 [0,7]; then choose P3 [7,8], then P2 [8,12]. Completion times 7,12,8. Turnaround P1=7,P2=10,P3=4. Waiting P1=0,P2=6,P3=3.

SRTF: P1 starts at 0; at t=2, P2 has 4 vs P1 remaining 5, so P2 preempts. At t=4, P3 arrives; P2 remaining 2, so P3 runs [4,5], then P2 [5,7], then P1 [7,12]. Completion P1=12,P2=7,P3=5. Turnaround 12,5,1; waiting 5,1,0. SRTF helps short jobs but delays P1.

Scheduling assumes CPU bursts/arrival times and ignores switch overhead unless stated. If two jobs tie, tie-breaking policy affects exact order.

**Exam method:** draw the ready set at every arrival/completion, mark remaining burst after preemption, then compute metrics from completion and first start.

## Extended lesson: compute scheduling metrics

For each process:
- Turnaround = completion time − arrival time.
- Waiting = turnaround − CPU burst time, when there is no I/O burst in the model.
- Response = first time scheduled − arrival time.

Use a Gantt chart to avoid mixing the measures. Example: P1 arrives 0 with burst 5; P2 arrives 1 with burst 2. FCFS runs P1 from 0–5, then P2 from 5–7. P1 completion=5, turnaround=5, waiting=0, response=0. P2 completion=7, turnaround=6, waiting=4, response=4. Average waiting=(0+4)/2=2.

Under non-preemptive SJF, the shortest available job starts next but runs to completion. Under SRTF, a newly arrived job can preempt when its remaining time is smaller. SJF/SRTF minimize average waiting under ideal known CPU bursts, but long jobs may starve. Round Robin gives each ready process a time quantum q; too large behaves like FCFS, too small causes high context-switch overhead.

### What each objective favors

- Throughput: jobs completed per unit time.
- Turnaround: completion latency for a job.
- Response time: time until first visible service.
- Fairness: avoid indefinite postponement.
A scheduler can improve one and worsen another. Interactive systems value response; batch systems may emphasize throughput/turnaround.

### Priority inversion

A high-priority thread can wait on a lock held by a low-priority thread while medium-priority work keeps preempting the lock holder. Priority inheritance temporarily raises the holder's priority so it can release the lock. Priority scheduling alone does not solve this synchronization problem.

### Practice

If a process arrives at 4, first runs at 7, and completes at 15 after using 6 CPU units: response=3, turnaround=11, waiting=5. Always use arrival time in both response and turnaround calculations.
