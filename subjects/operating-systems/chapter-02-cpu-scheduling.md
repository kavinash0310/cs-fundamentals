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