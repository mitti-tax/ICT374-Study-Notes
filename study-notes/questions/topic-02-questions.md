# Topic 02: Operating System Overview - Questions

[Back to summary](../notes/topic-02.md)

## Textbook
### Q1. What are three objectives of an OS design?
Source: Stallings Ch 2, Review Question 2.1, p. 127

### Q2. What is the kernel of an OS?
Source: Stallings Ch 2, Review Question 2.2, p. 127

### Q3. What is multiprogramming?
Source: Stallings Ch 2, Review Question 2.3, p. 127

### Q4. What is a process?
Source: Stallings Ch 2, Review Question 2.4, p. 127

### Q5. How is the execution context of a process used by the OS?
Source: Stallings Ch 2, Review Question 2.5, p. 127

### Q6. List and briefly explain five storage management responsibilities of a typical OS.
Source: Stallings Ch 2, Review Question 2.6, p. 127

### Q7. Explain the difference between a monolithic kernel and a microkernel.
Source: Stallings Ch 2, Review Question 2.9, p. 127

### Q8. What is multithreading?
Source: Stallings Ch 2, Review Question 2.10, p. 127

### Q9. What do you understand by a distributed operating system?
Source: Stallings Ch 2, Review Question 2.11, p. 127

### Q10. Suppose we have four jobs in a computer system, in the order JOB1, JOB2, JOB3 and JOB4. JOB1 requires 8 s of CPU time and 8 s of I/O time; JOB2 requires 4 s of CPU time and 14 s of disk time; JOB3 requires 6 s of CPU time; and, JOB4 requires 4 s of CPU time and 16 s of printer time. Define the following quantities for system utilization:
- Turnaround time = actual time to complete a job
- Throughput = average number of jobs completed per time period T
- Processor utilization = percentage of time that the processor is active (not waiting)

Compute these quantities (with illustrations if needed) in each of the following systems:
a. A uniprogramming system, whereby each job executes to completion before the next job can start its execution.
b. A multiprogramming system that follows a simple round-robin scheduling. Each process gets 2 s of CPU time turn-wise in a circular manner

Source: Stallings Ch 2, Problem 2.1, pp. 127-128

### Q11. In a batch operating system, three jobs are submitted for execution. Each job involves an I/O activity, CPU time and another I/O activity of the same time span as the first. Job JOB1 requires a total of 23 ms, with 3 ms CPU time; JOB2 requires a total time of 29 ms with 5 ms CPU time; JOB3 requires a total time of 14 ms with 4 ms CPU time. Illustrate their execution and find CPU utilization for uniprogramming and multiprogramming systems.
Source: Stallings Ch 2, Problem 2.2, p. 128

### Q12. Contrast the scheduling policies you might use when trying to optimize a time-sharing system with those you would use to optimize a multiprogrammed batch system.
Source: Stallings Ch 2, Problem 2.3, p. 128

### Q13. A computer system boots and starts a user application when an interrupt occurs. In which modes does the operating system work in this scenario?
Source: Stallings Ch 2, Problem 2.4, p. 128
