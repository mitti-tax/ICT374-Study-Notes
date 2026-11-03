# Topic 11: Concurrency and Parallelism - Questions

[Back to summary](../notes/topic-11.md)

## Textbook

### Q1. List reasons why a mode switch between threads may be cheaper than a mode switch between processes.
Source: Stallings Ch 4, Review Question 4.2, p. 218

### Q2. What are the two separate and potentially independent characteristics embodied in the concept of process?
Source: Stallings Ch 4, Review Question 4.3, p. 218

### Q3. How is a thread different from a process?
Source: Stallings Ch 4, Review Question 4.5, p. 218

### Q4. What are the advantages of using multithreading instead of multiple processes?
Source: Stallings Ch 4, Review Question 4.6, p. 218

### Q5. List some advantages and disadvantages of using kernel-level threads.
Source: Stallings Ch 4, Review Question 4.7, p. 218

### Q6. The use of multithreading improves the overall efficiency and performance of the execution of an application or program. However, not all programs are suitable for multithreading. Can you give some examples of programs where a multithreaded solution fails to improve on the performance of a single-threaded solution? Also give some examples where the performance improves when multiple threads are used in place of single threads.
Source: Stallings Ch 4, Problem 4.1, p. 218

### Q7. Suppose a program has a main routine that calls two sub-routines. The sub-routines can be executed in parallel. Give two possible approaches to implement this program, one using threads and the other without.
Source: Stallings Ch 4, Problem 4.2, p. 219

### Q8. Consider an environment in which there is a one-to-one mapping between user-level threads and kernel-level threads that allows one or more threads within a process to issue blocking system calls while other threads continue to run. Explain why this model can make multithreaded programs run faster than their single-threaded counterparts on a uniprocessor computer.
Source: Stallings Ch 4, Problem 4.4, p. 219

### Q9. Consider the following code using the POSIX Pthreads API:

```c
thread2.c
#include <pthread.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
int myglobal;

void *thread_function(void *arg) {
    int i,j;
    for ( i=0; i<20; i++ ) {
        j=myglobal;
        j=j+1;
        printf(".");
        fflush(stdout);
        sleep(1);
        myglobal=j;
    }
    return NULL;
}

int main(void) {
    pthread_t mythread;
    int i;
    if ( pthread_create( &mythread, NULL, thread_function, NULL) ) {
        printf("error creating thread.");
        abort();
    }
    for ( i=0; i<20; i++) {
        myglobal=myglobal+1;
        printf("o");
        fflush(stdout);
        sleep(1);
    }
    if ( pthread_join ( mythread, NULL ) ) {
        printf("error joining thread.");
        abort();
    }
    printf("\nmyglobal equals %d\n",myglobal);
    exit(0);
}
```

In main() we first declare a variable called mythread, which has a type of pthread_t. This is essentially an ID for a thread. Next, the if statement creates a thread associated with mythread. The call pthread_create() returns zero on success and a nonzero value on failure. The third argument of pthread_create() is the name of a function that the new thread will execute when it starts. When this thread_function() returns, the thread terminates. Meanwhile, the main program itself defines a thread, so there are two threads executing. The pthread_join function enables the main thread to wait until the new thread completes.

a. What does this program accomplish?
b. Here is the output from the executed program:

```
$ ./thread2
..o.o.o.o.oo.o.o.o.o.o.o.o.o.o..o.o.o.o.o
myglobal equals 21
```

Is this the output you would expect? If not, what has gone wrong?

Source: Stallings Ch 4, Problem 4.9, pp. 220-221

### Q10. What is a race condition?
Source: Stallings Ch 5, Review Question 5.3, p. 275

### Q11. What is starvation with respect to concurrency control by mutual exclusion?
Source: Stallings Ch 5, Review Question 5.7, p. 276

### Q12. What operations can be performed on a semaphore?
Source: Stallings Ch 5, Review Question 5.8, p. 276

### Q13. What is the difference between binary and general semaphores?
Source: Stallings Ch 5, Review Question 5.9, p. 276

### Q14. With respect to mutual exclusion using interrupt disabling
a. Mention the requirements for this exclusion and state which of them are met when interrupts are disabled.
b. Identify the problems associated with this mechanism.

Source: Stallings Ch 5, Problem 5.4, p. 276

### Q15. Consider the following processes P1 and P2 that update the value of the shared variables, x and y, as follows:

```
Process P1:                  Process P2:
( performs the operations:   ( performs the operations:
      x := x * y                   x ++
      y ++                         y := x * y
)                            )
LOAD R1, X                   LOAD R3, X
LOAD R2, Y                   INC R3
MUL R1, R2                   LOAD R4, Y
STORE X, R1                  MUL R4, R3
INC R2                       STORE X, R3
STORE Y, R2                  STORE Y, R4
```

Assume that the initial values of x and y are 2 and 3 respectively. P1 enters the system first and so it is required that the output is equivalent to a serial execution of P1 followed by P2. The scheduler in the uniprocessor system implements a pseudo-parallel execution of these two concurrent processes by interleaving their instructions without restricting the order of the interleaving.
a. If the processes P1 and P2 had executed serially, what would the values of x and y have been after the execution of both processes?
b. Write an interleaved concurrent schedule that gives the same output as a serial schedule.
c. Write an interleaved concurrent schedule that gives an output that is different from that of a serial schedule.

Source: Stallings Ch 5, Problem 5.6, p. 278

### Q16. Consider the following program:

```c
const int n = 50;
int tally;
void total()
{
  int count;
  for (count = 1; count<= n; count++) {
    tally++;
  }
}
void main()
{
  tally = 0;
  parbegin (total (), total ());
  write (tally);
}
```

a. Determine the proper lower bound and upper bound on the final value of the shared variable tally output by this concurrent program. Assume processes can execute at any relative speed, and a value can only be incremented after it has been loaded into a register by a separate machine instruction.
b. Suppose that an arbitrary number of these processes are permitted to execute in parallel under the assumptions of part (a). What effect will this modification have on the range of final values of tally?

Source: Stallings Ch 5, Problem 5.7, p. 278

### Q17. Give examples of reusable and consumable resources.
Source: Stallings Ch 6, Review Question 6.1, p. 332

### Q18. What are the three conditions that must be present for deadlock to be possible?
Source: Stallings Ch 6, Review Question 6.2, p. 332

### Q19. What are the four conditions that create deadlock?
Source: Stallings Ch 6, Review Question 6.3, p. 332

### Q20. How can the hold-and-wait condition be prevented?
Source: Stallings Ch 6, Review Question 6.4, p. 332

### Q21. Why can't you disallow mutual exclusion in order to prevent deadlocks?
Source: Stallings Ch 6, Review Question 6.5, p. 332

### Q22. How can the circular wait condition be prevented?
Source: Stallings Ch 6, Review Question 6.6, p. 332

### Q23. List some of the methods that may be adopted to recover from deadlocks.
Source: Stallings Ch 6, Review Question 6.7, p. 332

### Q24. Show that the four conditions of deadlock apply to Figure 6.1a.
Figure 6.1a is the "deadlock possible" crossing diagram (slide 17).

Source: Stallings Ch 6, Problem 6.1, p. 332

### Q25. Show how each of the techniques of prevention, avoidance, and detection can be applied to Figure 6.1.
Source: Stallings Ch 6, Problem 6.2, p. 332

### Q26. Given the following state of a system: The system comprises of five processes and four resources. P1-P5 denotes the set of processes. R1-R4 denotes the set of resources.

Total Existing Resources:

| R1 | R2 | R3 | R4 |
|----|----|----|----|
| 6 | 3 | 4 | 3 |

Snapshot at the initial time stage:

| | Alloc R1 | R2 | R3 | R4 | Claim R1 | R2 | R3 | R4 |
|---|---|---|---|---|---|---|---|---|
| P1 | 3 | 0 | 1 | 1 | 6 | 2 | 1 | 1 |
| P2 | 0 | 1 | 0 | 0 | 0 | 2 | 1 | 2 |
| P3 | 1 | 1 | 1 | 0 | 3 | 2 | 1 | 0 |
| P4 | 1 | 1 | 0 | 1 | 1 | 1 | 1 | 1 |
| P5 | 0 | 0 | 0 | 0 | 2 | 1 | 1 | 1 |

a. Compute the Available vector.
b. Compute the Need Matrix.
c. Is the current allocation state safe? If so, give a safe sequence of the process. In addition, show how the Available (working array) changes as each process terminates.
d. If the request (1, 1, 0, 0) from P1 arrives, will it be correct to grant the request? Justify your decision.

Source: Stallings Ch 6, Problem 6.5, pp. 332-333

### Q27. In the code below, three processes are competing for six resources labeled A to F.

```c
void P0()             void P1()             void P2()
{                     {                     {
 while (true) {        while (true) {        while (true) {
 get(A);               get(D);               get(C);
 get(B);               get(E);               get(F);
 get(C);               get(B);               get(D);
 // critical region:   // critical region:   // critical region:
 // use A, B, C        // use D, E, B        // use C, F, D
 release(A);           release(D);           release(C);
 release(B);           release(E);           release(F);
 release(C);           release(B);           release(D);
 }                     }                     }
}                     }                     }
```

a. Using a resource allocation graph (see Figures 6.5 and 6.6), show the possibility of a deadlock in this implementation.
b. Modify the order of some of the get requests to prevent the possibility of any deadlock. You cannot move requests across procedures, only change the order inside each procedure. Use a resource allocation graph to justify your answer.

Source: Stallings Ch 6, Problem 6.6, p. 333

### Q28. At an instant, the resource allocation state in a system is as follows: 4 processes P1-P4; 4 resource types: R1-R4; R1 (5 instances), R2 (3 instances), R3 (3 instances), R4 (3 instance)

Snapshot at time T0:

| | Alloc R1 | R2 | R3 | R4 | Request R1 | R2 | R3 | R4 |
|---|---|---|---|---|---|---|---|---|
| P1 | 0 | 0 | 1 | 0 | 2 | 0 | 0 | 2 |
| P2 | 2 | 0 | 0 | 1 | 1 | 3 | 0 | 1 |
| P3 | 0 | 1 | 1 | 0 | 2 | 1 | 1 | 0 |
| P4 | 1 | 1 | 0 | 0 | 4 | 0 | 3 | 1 |

Available: R1 2, R2 1, R3 1, R4 2

Run the deadlock detection algorithm and test whether the system is deadlocked or not. If it is, identify the processes that are deadlocked.

Source: Stallings Ch 6, Problem 6.11, p. 335

### Q29. Suppose the following two processes, foo and bar, are executed concurrently and share the semaphore variables S and R (each initialized to 1) and the integer variable x (initialized to 0).

```c
void foo( ) {                        void bar( ) {
  do {                                 do {
    semWait(S);                          semWait(R);
    semWait(R);                          semWait(S);
    x++;                                 x--;
    semSignal(S);                        semSignal(S);
    SemSignal(R);                        SemSignal(R);
  } while (1);                         } while (1);
}                                    }
```

a. Can the concurrent execution of these two processes result in one or both being blocked forever? If yes, give an execution sequence in which one or both are blocked forever.
b. Can the concurrent execution of these two processes result in the indefinite postponement of one of them? If yes, give an execution sequence in which one is indefinitely postponed.

Source: Stallings Ch 6, Problem 6.14, p. 335

### Q30. Consider a system consisting of four processes and 9 instances of a single resource. The current state of the claim (C) and allocation (A) matrices is:

C = (2, 6, 9, 5) and A = (1, 2, 2, 3), as column vectors for processes 1 to 4.

Is the system in a safe state? If so, will it remain in a safe state if the available resources are allocated to the last process in sequence?

Source: Stallings Ch 6, Problem 6.15, p. 336

### Q31. Consider the following ways of handling deadlock: (1) banker's algorithm, (2) detect deadlock and kill thread, releasing all resources, (3) reserve all resources in advance, (4) restart thread and release all resources if thread needs to wait, (5) resource ordering, and (6) detect deadlock and roll back thread's actions.
a. One criterion to use in evaluating different approaches to deadlock is which approach permits the greatest concurrency. In other words, which approach allows the most threads to make progress without waiting when there is no deadlock? Give a rank order from 1 to 6 for each of the ways of handling deadlock just listed, where 1 allows the greatest degree of concurrency. Comment on your ordering.
b. Another criterion is efficiency; in other words, which requires the least processor overhead. Rank order the approaches from 1 to 6, with 1 being the most efficient, assuming deadlock is a very rare event. Comment on your ordering. Does your ordering change if deadlocks occur frequently?

Source: Stallings Ch 6, Problem 6.16, p. 336

### Q32. Consider a variation of the dining philosophers problem where the number of philosophers is even. Can you devise a deadlock-free solution to the problem? Assume that all other requirements are like those in the original problem.
Source: Stallings Ch 6, Problem 6.17, p. 336
