# Topic 10: Processor Scheduling - Questions

[Back to summary](../notes/topic-10.md)

(The lecture slides and the sample exam paper contain no questions for this topic.)

## Textbook
### Q1. How is processor scheduling done in the batch portion of an OS?
Source: Stallings Ch 9, Review Question 9.1, p. 456

### Q2. What is the main function of a dispatcher? Give some examples of events when it is invoked.
Source: Stallings Ch 9, Review Question 9.2, p. 456

### Q3. What scheduling criteria affect the performance of a system?
Source: Stallings Ch 9, Review Question 9.3, p. 456

### Q4. If purely priority-based scheduling is used in a system, what are the problems that the system will face?
Source: Stallings Ch 9, Review Question 9.4, p. 456

### Q5. Identify the advantages and disadvantages of preemptive scheduling.
Source: Stallings Ch 9, Review Question 9.5, p. 456

### Q6. Briefly define FCFS scheduling.
Source: Stallings Ch 9, Review Question 9.6, p. 456

### Q7. Briefly define round-robin scheduling.
Source: Stallings Ch 9, Review Question 9.7, p. 456

### Q8. Briefly define shortest-process-next scheduling.
Source: Stallings Ch 9, Review Question 9.8, p. 456

### Q9. Briefly define shortest-remaining-time scheduling.
Source: Stallings Ch 9, Review Question 9.9, p. 456

### Q10. Briefly define highest-response-ratio-next scheduling.
Source: Stallings Ch 9, Review Question 9.10, p. 456

### Q11. Briefly define feedback scheduling.
Source: Stallings Ch 9, Review Question 9.11, p. 456

### Q12. Consider the following workload:

| Process | Burst Time | Priority | Arrival Time |
|---|---|---|---|
| P1 | 50 ms | 4 | 0 ms |
| P2 | 20 ms | 1 | 20 ms |
| P3 | 100 ms | 3 | 40 ms |
| P4 | 40 ms | 2 | 60 ms |

a. Show the schedule using shortest remaining time, nonpreemptive priority (a smaller priority number implies higher priority) and round robin with quantum 30 ms. Use time scale diagram as shown below for the FCFS example to show the schedule for each requested scheduling policy.
Example for FCFS (1 unit = 10 ms):

```
P1 P1 P1 P1 P1 P2 P2 P3 P3 P3 P3 P3 P3 P3 P3 P3 P3 P4 P4 P4 P4
0  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17 18 19 20
```

b. What is the average waiting time of the above scheduling policies?

Source: Stallings Ch 9, Problem 9.1, p. 456

### Q13. What factors determine the time quantum in round robin scheduling? Consider the system:

| Process | Arrival Time | Processing Time |
|---|---|---|
| P1 | 0 | 12 |
| P2 | 2 | 6 |
| P3 | 8 | 18 |
| P4 | 10 | 4 |

Context switch takes a time of 1 unit. Compute the average turnaround time of the processes for the time quanta q = 2, q = 4, and q = 8 respectively.

Source: Stallings Ch 9, Problem 9.2, p. 456

### Q14. Consider a variation of round robin scheduling, say NRR scheduling. In NRR scheduling, each process can have its own time quantum, q. The value of q starts out at 40 ms and decreases by 10 ms each time it goes through the round robin queue, until it reaches a minimum of 10 ms. Thus, long jobs get decreasingly shorter time slices. Analyze this scheduling algorithm for three jobs A, B, and C that arrive in the system having estimated burst times of 100 ms, 120 ms, and 60 ms respectively. Also identify some advantages and disadvantages that are associated with this algorithm.
Source: Stallings Ch 9, Problem 9.11, p. 458

### Q15. An interactive system using round-robin scheduling and swapping tries to give guaranteed response to trivial requests as follows. After completing a round-robin cycle among all ready processes, the system determines the time slice to allocate to each ready process for the next cycle by dividing a maximum response time by the number of processes requiring service. Is this a reasonable policy?
Source: Stallings Ch 9, Problem 9.13, p. 459

### Q16. Which type of process is generally favored by a multilevel feedback queueing scheduler: a processor-bound process, or an I/O-bound process? Briefly explain why.
Source: Stallings Ch 9, Problem 9.14, p. 459

### Q17. A variation of preemptive priority scheduling has dynamically changing priorities. A new process is assigned a priority 0. While a process is in the ready queue, its priority changes at the rate of a; and while it is executing, its priority changes at the rate of b. The different values of a and b give different algorithms. If larger values imply higher priorities, state with reasons the type of algorithm that will result if:
a. a < b < 0
b. b > a > 0

Source: Stallings Ch 9, Problem 9.15, p. 459

### Q18. Five batch jobs, A through E, arrive at a computer center at essentially the same time. They have an estimated running time of 15, 9, 3, 6, and 12 minutes, respectively. Their (externally defined) priorities are 6, 3, 7, 9, and 4, respectively, with a lower value corresponding to a higher priority. For each of the following scheduling algorithms, determine the turnaround time for each process and the average turnaround for all jobs. Ignore process switching overhead. Explain how you arrived at your answers. In the last three cases, assume only one job at a time runs until it finishes, and all jobs are completely processor bound.
a. round robin with a time quantum of 1 minute
b. priority scheduling
c. FCFS (run in order 15, 9, 3, 6, and 12)
d. shortest job first

Source: Stallings Ch 9, Problem 9.16, p. 459
