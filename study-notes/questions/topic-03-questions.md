# Topic 03: Process Description and Control - Questions

[Back to summary](../notes/topic-03.md)

## Textbook
### Q1. What is an instruction trace?
Source: Stallings Ch 3, Review Question 3.1, p. 172

### Q2. Explain the concept of a process and mark its differences from a program.
Source: Stallings Ch 3, Review Question 3.2, p. 172

### Q3. For the processing model of Figure 3.6, briefly define each state.
Source: Stallings Ch 3, Review Question 3.3, p. 172

### Q4. What does it mean to preempt a process?
Source: Stallings Ch 3, Review Question 3.4, p. 172

### Q5. What is process spawning?
Source: Stallings Ch 3, Review Question 3.5, p. 172

### Q6. Why does Figure 3.9b have two blocked states?
Source: Stallings Ch 3, Review Question 3.6, p. 172

### Q7. List four characteristics of a suspended process.
Source: Stallings Ch 3, Review Question 3.7, p. 172

### Q8. For what types of entities does the OS maintain tables of information for management purposes?
Source: Stallings Ch 3, Review Question 3.8, p. 172

### Q9. What are the elements of a process image?
Source: Stallings Ch 3, Review Question 3.9, p. 172

### Q10. Why are two modes (user and kernel) needed?
Source: Stallings Ch 3, Review Question 3.10, p. 172

### Q11. What are the steps performed by an OS to create a new process?
Source: Stallings Ch 3, Review Question 3.11, p. 172

### Q12. What is the difference between an interrupt and a trap?
Source: Stallings Ch 3, Review Question 3.12, p. 172

### Q13. Give three examples of an interrupt.
Source: Stallings Ch 3, Review Question 3.13, p. 172

### Q14. What is the difference between a mode switch and a process switch?
Source: Stallings Ch 3, Review Question 3.14, p. 172

### Q15. Suppose that four interleaved processes are running in a system having start addresses 4050, 3200, 5000 and 6700. The traces of the individual processes are as follows:

| Process P1 | Process P2 | Process P3 | Process P4 |
|---|---|---|---|
| 4050 | 3200 | 5000 | 6700 |
| 4051 | 3201 | 5001 | 6701 |
| 4052 | 3202 | 5002 | 6702 |
| 4053 | 3203 | 5003 | `<I/O>` |
| 4054 | 3204 | 5004 | |
| 4055 | 3205 | 5005 | |
| 4056 | 3206 | 5006 | |
| 4057 | `<I/O>` | 5007 | |
| 4058 | | 5008 | |
| 4059 | | 5009 | |
| 4060 | | 5010 | |

Find the interleaved traces of the processes. Assume that the dispatcher is invoked after 5 instructions or for interrupts and the dispatcher cycle has 4 instructions.
Source: Stallings Ch 3, Problem 3.2, pp. 172-173

### Q16. Figure 3.9b contains seven states. In principle, one could draw a transition between any two states, for a total of 42 different transitions.
a. List all of the possible transitions and give an example of what could cause each transition.
b. List all of the impossible transitions and explain why.
Source: Stallings Ch 3, Problem 3.3, p. 173 (refers to Figure 3.9b, slide 25)

### Q17. For the seven-state process model of Figure 3.9b, draw a queueing diagram similar to that of Figure 3.8b.
Source: Stallings Ch 3, Problem 3.4, p. 173 (Figure 3.9b on slide 25; Figure 3.8b on slide 22)

### Q18. Consider the state transition diagram of Figure 3.9b. Suppose it is time for the OS to dispatch a process and there are processes in both the Ready state and the Ready/Suspend state, and at least one process in the Ready/Suspend state has higher scheduling priority than any of the processes in the Ready state. Two extreme policies are as follows: (1) Always dispatch from a process in the Ready state, to minimize swapping, and (2) always give preference to the highest-priority process, even though that may mean swapping when swapping is not necessary. Suggest an intermediate policy that tries to balance the concerns of priority and performance.
Source: Stallings Ch 3, Problem 3.5, p. 173 (refers to Figure 3.9b, slide 25)

### Q19. Figure 3.8b suggests that a process can only be in one event queue at a time.
a. Is it possible that you would want to allow a process to wait on more than one event at the same time? Provide an example.
b. In that case, how would you modify the queueing structure of the figure to support this new feature?
Source: Stallings Ch 3, Problem 3.9, p. 175 (Figure 3.8b on slide 22)

### Q20. What is the purpose of the system call fork() in the UNIX operating system? Write a C routine to create a child process using the fork() system call. Incorporate an error check in your routine in case the creation of the child process fails.
Source: Stallings Ch 3, Problem 3.10, p. 175

### Q21. You have executed the following C program:

```c
main ()
{ int pid;
  pid = fork ();
  printf ("%d \n", pid);
}
```

What are the possible outputs, assuming the fork succeeded?
Source: Stallings Ch 3, Problem 3.12, p. 175

## Sample exam
### Q22. The Process Image element that contains the modifiable part of the user space is called the:
a. User Program
b. System Stack
c. Process Control Block
d. None of the above
Source: Sample exam, Part I, Q3

### Q23. A process switch may occur when the system encounters an interrupt condition, such as that generated by a:
a. Memory fault
b. Supervisor call
c. Trap
d. All of the above
e. None of the above
Source: Sample exam, Part I, Q4
