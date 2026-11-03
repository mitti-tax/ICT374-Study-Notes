# Topic 11: Concurrency and Parallelism

## Overview
Concurrency means several execution flows exist at the same time and often share resources. On one processor they interleave; on several processors they can run in parallel. This brings efficiency but also race conditions, deadlock, starvation and livelock, which the lecture handles with mutual exclusion, semaphores and deadlock strategies. The second half covers threads (user-level vs kernel-level), Flynn's taxonomy, parallel computing, SMP and microkernels.

## Concurrency
- More than one execution flow exists at once; flows share resources (slide 4).
- Benefit: more efficient use of time, processor, memory and I/O devices. Every modern OS is concurrent (kernel plus many processes at once).
- Difficulties: sharing global resources, allocating resources optimally, and locating programming errors (they are timing dependent and hard to reproduce).

### Problems with concurrency
- **Race condition**: two or more processes read and write shared data and the result depends on relative timing.
- **Deadlock**: processes wait for each other and none can proceed.
- **Starvation**: a runnable process is indefinitely overlooked by the scheduler.
- **Livelock**: processes keep changing state in response to each other without doing useful work.

### Race condition example
`echo()` uses globals `chin` and `chout` (`chin = getchar(); chout = chin; putchar(chout);`). If P1 reads a char, then P2 runs and overwrites `chin` before P1 copies it, P1 prints P2's character and P1's input is lost. The final output depends on interleaving.

## Mutual exclusion
- **Critical section**: code that accesses shared resources and must not run while another process is in its corresponding section.
- **Mutual exclusion**: only one process at a time in its critical section (e.g. one process at a time sending commands to the printer).
- Requirements: one process at a time; a process halting in its non-critical section must not interfere with others; no deadlock or starvation; no delay when no one else is in the section; no assumptions about relative speeds or number of processes; a process stays inside for a finite time.
- **Hardware support**: interrupt disabling (so nothing preempts the critical section; dangerous if the code has a bug) and special machine instructions, **test-and-set** and **exchange**.
- **Semaphores**: a shared binary semaphore `s`; each process does `P(s)`, uses `chin`/`chout`, then `V(s)`. This fixes the echo race. Semaphores are the usual user-level tool for this.

## Deadlock
Permanent blocking of a set of processes that compete for resources or communicate. There is no efficient general solution. It often results from enforcing mutual exclusion on shared resources.
- Slide 17 (Figure 6.1): four cars at a crossing, each needing the lane held by the next; with all four in, nobody can move.
- **Reusable resources**: used by one process at a time and not depleted (processors, I/O channels, memory, files, databases, semaphores). Deadlock if each holds one and requests the other. Slide 19 (Figure 6.4): P requests D then T, Q requests T then D; if each locks its first, both wait forever. Slide 20: 200 KB of memory, P1 and P2 each request 80/70 KB then 60/80 KB; deadlock if both reach the second request.
- **Consumable resources**: created and destroyed (interrupts, signals, messages, I/O buffer data). Deadlock if `Receive` blocks: P1 `Receive(P2)` before `Send(P2,M1)` and P2 `Receive(P1)` before `Send(P1,M2)`. May need a rare combination of events.
- **Resource allocation graph**: directed graph of processes and resources. An arrow resource-to-process means "held by"; process-to-resource means "requests". Slide 24: a cycle P1 -> Ra -> P2 -> Rb -> P1 with single-unit resources is a circular wait; with multiple units per resource (dots) the same shape is not a deadlock. Slide 27 (Figure 6.6): the graph for the crossing, a cycle through P1-P4.

![Fig 6.1 Illustration of deadlock (slide 17)](../images/topic-11/slide-17.png)

![Fig 6.4 Two processes competing for reusable resources (slide 19)](../images/topic-11/slide-19.png)

![Fig 6.5c,d Resource allocation graphs (slide 24)](../images/topic-11/slide-24.png)

![Fig 6.6 Resource allocation graph for Fig 6.1b (slide 27)](../images/topic-11/slide-27.png)

### Conditions for deadlock
Necessary: mutual exclusion, hold-and-wait, no preemption. Adding **circular wait** (closed chain where each process holds a resource needed by the next) makes the set necessary and sufficient.

![Fig 6.5c Circular wait (slide 26)](../images/topic-11/slide-26.png)

### Deadlock prevention (break one condition)
- Mutual exclusion: don't enforce it where not needed (reading a shared file); not always possible.
- Hold and wait: require all resources be requested at once.
- No preemption: process releases and re-requests, or OS preempts it.
- Circular wait: define a linear ordering of resource types.

### Deadlock avoidance
Decide dynamically whether granting a request could lead to deadlock; needs knowledge of future requests. Two approaches: don't start a process whose demands might lead to deadlock; don't grant an incremental request that might lead to deadlock.
- **Banker's algorithm**: the state is the current allocation. A **safe state** has at least one sequence in which all processes can finish; an **unsafe state** is not safe (unsafe does not mean deadlocked yet). Grant a request only if the resulting state is safe.

### Deadlock detection and recovery
Once detected: abort all deadlocked processes; roll back each to a checkpoint and restart (the deadlock may recur); abort them one by one until it clears; or preempt resources one by one until it clears.

### Dining philosophers (slide 34)
Five philosophers share five forks around a table; each needs two forks to eat. If all grab the left fork, deadlock; unlucky timing can also starve a philosopher.

![Fig 6.11 Dining arrangement for philosophers (slide 34)](../images/topic-11/slide-34.png)

## UNIX concurrency mechanisms
Pipes, messages, shared memory, semaphores, signals.

## Threads
### Process vs thread
A traditional process has two aspects, treated independently by the OS:
- **Resource ownership**: a virtual address space holding the process image (the "process" or "task").
- **Scheduling/execution**: an execution path interleaved with others (the "thread").

### Multithreading
The OS supports multiple threads in one process. Slide 42 (Fig 4.2): a single-threaded process has one PCB, user address space, user stack and kernel stack; a multithreaded process has one PCB and address space, plus a thread control block, user stack and kernel stack per thread. MS-DOS: one thread. Traditional UNIX: many processes, one thread each. Windows, Solaris, Linux, Mach, OS/2, macOS: multiple threads per process. Slide 39 (Figure 4.1) shows the four combinations: one process/one thread, one process/many threads, many processes/one thread each, many processes/many threads each.
- **Process** has a virtual address space and protected access to processors, other processes, files and I/O.
- **Thread** has an execution state, saved context when not running, its own stack, some per-thread static storage for locals, and access to the memory and resources of its process (shared by all its threads).

![Fig 4.1 Threads and processes (slide 39)](../images/topic-11/slide-39.png)

![Fig 4.2 Single-threaded and multithreaded process models (slide 42)](../images/topic-11/slide-42.png)

### Benefits
Faster to create and to terminate than a process; faster to switch between threads of the same process; threads share memory and files so they communicate without calling the kernel.
Suspending a process suspends all its threads (shared address space); terminating a process terminates all threads.
Thread states/operations: **spawn**, **block**, **unblock**, **finish** (deallocate register context and stacks).
Example (slides 46-47): an RPC to two servers. Single thread waits for the servers one after the other; with threads both calls run at once, so total time is shorter.

![Fig 4.3a RPC using a single thread (slide 46)](../images/topic-11/slide-46.png)

![Fig 4.3b RPC using one thread per server (slide 47)](../images/topic-11/slide-47.png)

![Fig 4.4 Multithreading on a uniprocessor (slide 48)](../images/topic-11/slide-48.png)

### User-level threads (ULT)
All thread management is in the application (a thread library); the kernel is unaware of the threads and schedules only the process. Switching needs no kernel mode switch, but if one thread blocks the whole process blocks, and threads of one process cannot run on multiple processors at once. Slide 51 (Fig 4.7) shows that thread state and process state are separate: when the running thread makes a blocking system call (b), the process is Blocked while the thread still shows Running; when the time slice ends (c), the process is Ready while the thread still shows Running; a thread can be Blocked waiting for another thread while the process stays Running (d).

![User-level threads (slide 50)](../images/topic-11/slide-50.png)

![Fig 4.7 Thread states and process states (user-level threads) (slide 51)](../images/topic-11/slide-51.png)

### Kernel-level threads (KLT)
Windows is the example. The kernel keeps context for the process and for each thread and schedules on a per-thread basis. Threads of one process can run in parallel on a multiprocessor and one blocking does not block the others, but switching costs a mode switch.

![Pure kernel-level threads (slide 53)](../images/topic-11/slide-53.png)

### Combined approaches
Solaris is the example: threads are created in user space, and most scheduling and synchronization of threads is done within the application, mapped onto kernel threads.

![Combined user-level and kernel-level threads (slide 55)](../images/topic-11/slide-55.png)

### Threads:processes relationships (slide 56, Table 4.2)
1:1 traditional UNIX; M:1 Windows NT, Solaris, Linux, OS/2, OS/390, MACH (a process owns the address space, many threads); 1:M Clouds, Emerald (thread migrates between process environments); M:N TRIX.

## Flynn's taxonomy
- **SISD**: one processor, one instruction stream, data in one memory.
- **SIMD**: each instruction executes on different data by different processors. NVIDIA GPUs use a similar model called SIMT (single instruction, multiple threads).
- **MISD**: one data sequence goes to processors each running a different instruction sequence; never implemented.
- **MIMD**: processors simultaneously run different instruction sequences on different data.
Slide 59 (Figure 4.8): parallel processors split into SIMD and MIMD; MIMD splits into shared-memory (tightly coupled: master/slave, SMP) and distributed-memory (loosely coupled: clusters).

![Fig 4.8 Parallel processor architectures (slide 59)](../images/topic-11/slide-59.png)

## Symmetric multiprocessing (SMP)
Slide 61 (Fig 4.9): several processors, each with an L1 and an L2 cache, share the system bus with main memory and the I/O subsystem (I/O adapters). The kernel can run on any processor, and typically each processor self-schedules from the pool of available processes or threads. Design considerations (slide 63): simultaneous concurrent processes/threads, scheduling, synchronization, memory management, reliability and fault tolerance.

![Fig 4.9 Symmetric multiprocessor organization (slide 61)](../images/topic-11/slide-61.png)

## Parallel computing
Use multiple processors to solve one task by splitting the program into parts that run in parallel. The main aim is to shorten execution time.

## Microkernels
A small OS core with only essential functions; services such as device drivers, file systems, virtual memory manager, windowing and security run as external subsystems (slide 65, Figure 4.10: layered kernel with all services in kernel mode versus microkernel in kernel mode with servers as user-mode processes).
Benefits:
- Uniform interface: kernel and user services are all reached by message passing.
- Extensibility and flexibility: add or remove features.
- Portability: porting changes are mostly in the microkernel.
- Reliability: modular, and a small kernel can be rigorously tested.
- Distributed system support: messages are sent without knowing the target machine.
- Object-oriented OS: components are objects with defined interfaces.
Examples: Windows, macOS (Darwin); Linux is a notable exception.

![Fig 4.10 Kernel architecture: layered kernel and microkernel (slide 65)](../images/topic-11/slide-65.png)

## Key terms
concurrency, parallelism, race condition, critical section, mutual exclusion, test-and-set, semaphore, deadlock, livelock, starvation, reusable/consumable resource, resource allocation graph, hold-and-wait, circular wait, prevention/avoidance/detection, safe/unsafe state, banker's algorithm, dining philosophers, thread, user-level thread, kernel-level thread, SISD/SIMD/MISD/MIMD, SMP, microkernel.

## Questions
[Practice questions](../questions/topic-11-questions.md)

## Sources
- Lecture slides: ICT374 Topic 11, slides 1-69 (concurrency 4-15, deadlock 16-34, UNIX mechanisms 35, threads 36-56, Flynn/SMP/parallel 57-63, microkernels 64-69).
- Stallings Ch 4.1-4.3 (pp. 177-194), Ch 5.1-5.2 (pp. 232-246), Ch 6.1-6.4 (pp. 290-308); Ch 1.8 (pp. 54-57) and Ch 2.4/2.6 (pp. 92-94, 98-100) for SMP and microkernel background. Figures on slides 17, 19, 24, 27, 34, 39, 56, 59, 65 are from Stallings Figs 6.1, 6.4, 6.5, 6.6, 6.11, 4.1, Table 4.2, Fig 4.8, Fig 4.10.
