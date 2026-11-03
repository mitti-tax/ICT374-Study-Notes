# Topic 10: Processor Scheduling

## Overview
Scheduling assigns processes to run on the processor(s). It is judged by response time, throughput and processor efficiency. The lecture covers the three levels of scheduling (long, medium, short term), the criteria for short-term scheduling, priorities and preemption, six short-term policies (FCFS, Round-Robin, SPN, SRT, HRRN, Feedback) and the traditional UNIX scheduler.

## Aim of scheduling (slide 2)
Assign processes to be executed by the processor(s), trading off response time, throughput and processor efficiency.

## Types of scheduling (slides 3-5, 12)
| Type | Decision |
|---|---|
| Long-term | add to the pool of processes to be executed (New -> Ready or Ready/Suspend) |
| Medium-term | add to the number of processes partially or fully in main memory (swapping; Suspend <-> Ready/Blocked) |
| Short-term | which ready process the processor runs next (Ready -> Running) |
| I/O | which process's pending I/O request an available device handles |

- Fig 9.1 (slide 4) shows these as transitions in the process state diagram. Fig 9.2 (slide 5) nests them: short-term covers Running/Ready/Blocked, medium-term adds the two Suspend states, long-term adds New and Exit.
- Fig 9.3 (slide 12) is the queuing diagram: batch jobs and interactive users enter via long-term scheduling, the Ready queue feeds the processor (short-term), time-out returns a process to Ready, event wait sends it to the Blocked queue, and medium-term scheduling moves processes among Ready/Suspend and Blocked/Suspend queues.

### Long-term scheduling (slide 6)
Decides which programs are admitted to the system. It controls the degree of multiprogramming: more processes means each gets a smaller share of processor time.

### Medium-term scheduling (slide 7)
Part of the swapping function, driven by the need to manage the degree of multiprogramming.

### Short-term scheduling (slide 8)
Also called the **dispatcher**. It runs most frequently, and is invoked when an event occurs: clock interrupts, I/O interrupts, OS calls, signals.

## Short-term scheduling criteria (slides 9-11)
- **User-oriented**: response time (time from submitting a request until output begins), turnaround time, deadlines, predictability.
- **System-oriented**: effective and efficient processor use: throughput, processor utilization, fairness, enforcing priorities, balancing resources.
- **Performance-related** criteria are quantitative and measurable (response time, throughput); others are qualitative.
- Turnaround time = submission to completion (execution plus waiting); good for batch jobs. Throughput = processes completed per unit time.

## Priorities (slides 13-14)
- The scheduler always picks a ready process of higher priority over a lower one.
- Fig 9.4: multiple ready queues RQ0..RQn, one per priority level; plus a Blocked queue. Processes are dispatched from the highest non-empty queue.
- Lower-priority processes may **starve**. Fix: let priority change with age or execution history.

## Decision mode (slide 15)
- **Nonpreemptive**: a running process continues until it terminates or blocks for I/O.
- **Preemptive**: the OS may interrupt the running process and move it to Ready. Better service since no process can monopolize the processor.

## Process scheduling example (slide 16)
Table 9.4 is the running example used to compare policies:

| Process | Arrival | Service |
|---|---|---|
| A | 0 | 3 |
| B | 2 | 6 |
| C | 4 | 4 |
| D | 6 | 5 |
| E | 8 | 2 |

## Scheduling policies (slides 17-26)
Notation: w = time spent waiting, e = time executed so far, s = total service time required (including e).

### First-Come-First-Served (FCFS) (slides 17-18)
Each process joins the Ready queue; when the current one stops, the oldest is chosen. Nonpreemptive. A short process may wait a very long time behind a long one. Favors CPU-bound processes: I/O-bound processes wait for the CPU-bound one to finish. Selection function max[w].

### Round-Robin (slides 19-21)
- Preemption based on a clock: each process gets a fixed time quantum. A periodic clock interrupt puts the running process at the back of the Ready queue and the next ready job is selected. This is **time slicing**.
- If the quantum is too small, throughput suffers (overhead of switching). Gives fair treatment and good response for short processes.
- **Virtual Round-Robin** (Fig 9.7): processes that finish I/O go to an **Auxiliary queue** that has preference over the Ready queue (this removes round-robin's bias against I/O-bound processes).

### Shortest Process Next (SPN) (slide 22)
Nonpreemptive. Pick the process with the shortest expected processing time (min[s]). Short processes jump ahead. Needs an estimate of processing time; longer processes may starve.

### Shortest Remaining Time (SRT) (slide 23)
Preemptive version of SPN: pick min[s - e], and may preempt when a new process arrives. Needs a time estimate.

### Highest Response Ratio Next (HRRN) (slide 24)
Nonpreemptive. Choose the process with the greatest ratio:

ratio = (time spent waiting + expected service time) / expected service time = (w + s) / s

Short jobs are favored, but a waiting process's ratio grows, so no starvation.

### Multilevel Feedback (slides 25-26)
Penalizes jobs that have been running longer, used when remaining time is unknown. Fig 9.10: ready queues RQ0..RQn; a new process enters RQ0, and each time it is preempted it drops to the next lower queue (the last queue cycles round-robin). Preemptive at the time quantum; may favor I/O-bound processes; starvation possible.

### Comparison (Table 9.3, slide 26)
| Policy | Selection | Mode | Throughput | Response time | Overhead | Effect | Starvation |
|---|---|---|---|---|---|---|---|
| FCFS | max[w] | Nonpreemptive | Not emphasized | May be high, esp. with large variance in run times | Minimum | Penalizes short and I/O-bound | No |
| Round Robin | constant | Preemptive (quantum) | May be low if quantum too small | Good for short processes | Minimum | Fair | No |
| SPN | min[s] | Nonpreemptive | High | Good for short processes | Can be high | Penalizes long | Possible |
| SRT | min[s-e] | Preemptive (at arrival) | High | Good | Can be high | Penalizes long | Possible |
| HRRN | max[(w+s)/s] | Nonpreemptive | High | Good | Can be high | Good balance | No |
| Feedback | (see text) | Preemptive (quantum) | Not emphasized | Not emphasized | Can be high | May favor I/O-bound | Possible |

## Traditional UNIX scheduling (slides 27-28)
- Multilevel feedback, using round robin within each priority queue.
- A running process that does not block or complete within 1 second is preempted.
- Priorities are recomputed once per second. (Textbook, p. 453: CPU_j(i) = CPU_j(i-1)/2 and P_j(i) = Base_j + CPU_j(i)/2 + nice_j; lower value = higher priority; this penalizes CPU-bound processes.)
- The **base priority** divides processes into fixed bands; CPU and nice terms cannot move a process out of its band.
- Bands, decreasing priority: Swapper, Block I/O device control, File manipulation, Character I/O device control, User processes.

## Key terms
dispatcher; degree of multiprogramming; long/medium/short-term scheduler; response time; turnaround time; throughput; processor utilization; fairness; starvation; preemptive / nonpreemptive; time quantum / time slicing; auxiliary queue; FCFS; Round-Robin; SPN; SRT; HRRN; multilevel feedback; base priority; nice value; priority bands.

## Questions
[Practice questions](../questions/topic-10-questions.md)

## Sources
- Slides: ICT374Topic10, slides 1-28 (slides 3-5, 10-12, 14, 16, 21, 25, 26 are figures/tables viewed as images).
- Stallings Ch 9: 9.1 Types of Processor Scheduling, 9.2 Scheduling Algorithms, 9.3 Traditional UNIX Scheduling, pp. 426-454 (UNIX formula: p. 453).
