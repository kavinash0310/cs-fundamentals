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
