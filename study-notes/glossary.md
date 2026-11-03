# Glossary

Merged from the Key terms sections of the 11 topic notes. Entries with a definition are copied from the notes; bare entries are terms the notes list without a definition, so follow the topic link. Topic numbers link to the note.


## A

- **adopted by init** ([T4](notes/topic-04.md))
- **Asynchronous event**: may happen at an unpredictable time. ([T6](notes/topic-06.md))
- **auxiliary queue** ([T10](notes/topic-10.md))

## B

- **banker's algorithm** ([T11](notes/topic-11.md))
- **base priority** ([T10](notes/topic-10.md))
- **Base/bounds registers**: hardware relocation and protection. ([T9](notes/topic-09.md))
- **batch** ([T2](notes/topic-02.md))
- **Blocked**: delivery postponed; signal stays pending. ([T6](notes/topic-06.md))
- **Byte stream / message-oriented**: no boundaries (TCP) vs boundaries preserved (UDP). ([T8](notes/topic-08.md))

## C

- **Cache hit / miss**: needed word is / is not in cache. ([T1](notes/topic-01.md))
- **chdir** ([T4](notes/topic-04.md))
- **circular wait** ([T11](notes/topic-11.md))
- **Compaction**: shifting processes to merge free space. ([T9](notes/topic-09.md))
- **concurrency** ([T11](notes/topic-11.md))
- **Concurrent vs iterative server**: forks a child per client vs serves one at a time. ([T8](notes/topic-08.md))
- **Connection-oriented / connectionless**: TCP vs UDP service models. ([T8](notes/topic-08.md))
- **control terminal** ([T4](notes/topic-04.md))
- **critical section** ([T11](notes/topic-11.md))
- **CTSS** ([T2](notes/topic-02.md))

## D

- **daemon**: background process detached from the terminal (`fork`, `setsid`, `chdir("/")`, `umask(0)`). ([T4](notes/topic-04.md), [T8](notes/topic-08.md))
- **deadlock** ([T11](notes/topic-11.md))
- **degree of multiprogramming** ([T10](notes/topic-10.md))
- **dining philosophers** ([T11](notes/topic-11.md))
- **directory file** ([T5](notes/topic-05.md))
- **Disk cache**: main-memory buffer for disk data. ([T1](notes/topic-01.md))
- **dispatcher** ([T3](notes/topic-03.md), [T10](notes/topic-10.md))
- **distributed OS** ([T2](notes/topic-02.md))

## E

- **effective uid/gid** ([T5](notes/topic-05.md))
- **environ** ([T4](notes/topic-04.md))
- **environment** ([T4](notes/topic-04.md))
- **envp** ([T4](notes/topic-04.md))
- **exec family** ([T4](notes/topic-04.md))
- **execution context** ([T2](notes/topic-02.md))
- **executive** ([T2](notes/topic-02.md))
- **execve** ([T4](notes/topic-04.md))
- **exit/_exit** ([T4](notes/topic-04.md))

## F

- **fairness** ([T10](notes/topic-10.md))
- **FCFS** ([T10](notes/topic-10.md))
- **FD_CLOEXEC** ([T5](notes/topic-05.md))
- **FIFO / named pipe**: pipe with a file name (`mkfifo`). ([T7](notes/topic-07.md))
- **file descriptor** ([T5](notes/topic-05.md))
- **File Table** ([T5](notes/topic-05.md))
- **filter** ([T5](notes/topic-05.md))
- **fork** ([T4](notes/topic-04.md))
- **Frame / page / segment**: physical fixed block / process fixed block / variable-size logical unit. ([T9](notes/topic-09.md))

## G

- **Generated, delivered, pending**: stages of a signal's life. ([T6](notes/topic-06.md))
- **getenv** ([T4](notes/topic-04.md))
- **group leader** ([T4](notes/topic-04.md))

## H

- **HAL** ([T2](notes/topic-02.md))
- **hard link** ([T5](notes/topic-05.md))
- **hold-and-wait** ([T11](notes/topic-11.md))
- **HRRN** ([T10](notes/topic-10.md))

## I

- **i-list** ([T5](notes/topic-05.md))
- **i-node** ([T5](notes/topic-05.md))
- **INADDR_ANY**: bind to all local network interfaces. ([T8](notes/topic-08.md))
- **init** ([T4](notes/topic-04.md))
- **Internal fragmentation**: waste inside an allocated partition/page. **External fragmentation**: scattered free holes between partitions. ([T9](notes/topic-09.md))
- **Interrupt**: signal that suspends normal instruction sequencing. ([T1](notes/topic-01.md), [T3](notes/topic-03.md))
- **Interrupt handler**: OS routine servicing an interrupt. ([T1](notes/topic-01.md))
- **ipcs / ipcrm**: list and remove IPC objects. ([T7](notes/topic-07.md))

## K

- **kernel mode** ([T2](notes/topic-02.md), [T3](notes/topic-03.md))
- **kernel-level thread** ([T11](notes/topic-11.md))
- **kernel/nucleus** ([T2](notes/topic-02.md))
- **Key / id**: user-chosen or system-chosen name of an IPC object, and the id handed out at creation. `IPC_PRIVATE`, `IPC_CREAT`, `IPC_EXCL` are the related constants. ([T7](notes/topic-07.md))

## L

- **link count** ([T5](notes/topic-05.md))
- **livelock** ([T11](notes/topic-11.md))
- **Locality**: references cluster in time and space. ([T9](notes/topic-09.md))
- **Locality of reference**: accesses cluster in time and space, so a small fast memory is effective. ([T1](notes/topic-01.md))
- **log file** ([T4](notes/topic-04.md))
- **long/medium/short-term scheduler** ([T10](notes/topic-10.md))

## M

- **major/minor device number** ([T5](notes/topic-05.md))
- **MAR / MBR**: address and data buffer registers for memory access. ([T1](notes/topic-01.md))
- **Memory hierarchy**: levels trading speed against cost and capacity. ([T1](notes/topic-01.md))
- **memory image** ([T4](notes/topic-04.md))
- **Message type (mtype)**: positive tag that selects which message `msgrcv` returns. ([T7](notes/topic-07.md))
- **microkernel** ([T2](notes/topic-02.md), [T11](notes/topic-11.md))
- **mode switch** ([T3](notes/topic-03.md))
- **monitor** ([T2](notes/topic-02.md))
- **multilevel feedback** ([T10](notes/topic-10.md))
- **multiprogramming** ([T2](notes/topic-02.md))
- **multithreading** ([T2](notes/topic-02.md))
- **mutual exclusion** ([T11](notes/topic-11.md))

## N

- **Network byte order**: big-endian; `htons/htonl/ntohs/ntohl`. ([T8](notes/topic-08.md))
- **nice value** ([T10](notes/topic-10.md))

## O

- **O_NONBLOCK / EAGAIN**: non-blocking mode and its "would block" error. ([T7](notes/topic-07.md))
- **OPT, LRU, FIFO, Clock**: replacement algorithms. ([T9](notes/topic-09.md))

## P

- **Page fault**: referenced page is not in main memory. ([T9](notes/topic-09.md))
- **Page table / segment table / inverted page table**: translation structures. ([T9](notes/topic-09.md))
- **paging** ([T2](notes/topic-02.md))
- **parallelism** ([T11](notes/topic-11.md))
- **parent/child** ([T3](notes/topic-03.md), [T4](notes/topic-04.md))
- **PC / IR / PSW**: next-instruction address / current instruction / status and mode bits. ([T1](notes/topic-01.md))
- **PCB** ([T3](notes/topic-03.md), [T4](notes/topic-04.md))
- **per-process open-file table** ([T5](notes/topic-05.md))
- **PID** ([T4](notes/topic-04.md))
- **Pipe / unnamed pipe**: kernel FIFO buffer between related processes (`pipe`). ([T7](notes/topic-07.md))
- **preempt** ([T3](notes/topic-03.md))
- **preemptive / nonpreemptive** ([T10](notes/topic-10.md))
- **Present (P), modify (M), use bit**: control bits in entries. ([T9](notes/topic-09.md))
- **prevention/avoidance/detection** ([T11](notes/topic-11.md))
- **priority bands** ([T10](notes/topic-10.md))
- **privileged instruction** ([T2](notes/topic-02.md))
- **process** ([T2](notes/topic-02.md), [T3](notes/topic-03.md), [T4](notes/topic-04.md))
- **process group** ([T4](notes/topic-04.md))
- **process image** ([T3](notes/topic-03.md))
- **process switch** ([T3](notes/topic-03.md))
- **process table** ([T3](notes/topic-03.md))
- **processor utilization** ([T10](notes/topic-10.md))
- **Producer-consumer**: classic synchronisation problem, solved with `empty` and `full`. ([T7](notes/topic-07.md))
- **Programmed I/O, interrupt-driven I/O, DMA**: the three ways of doing I/O, in increasing order of CPU relief. ([T1](notes/topic-01.md))
- **PSW** ([T3](notes/topic-03.md))

## R

- **race condition** ([T11](notes/topic-11.md))
- **readn / writen**: length-prefixed read and write loops that restore message boundaries over TCP. ([T8](notes/topic-08.md))
- **redirection** ([T5](notes/topic-05.md))
- **Reentrant-safe**: safe to call again while already executing. ([T6](notes/topic-06.md))
- **Resident set**: pages of a process in main memory. ([T9](notes/topic-09.md))
- **resource allocation graph** ([T11](notes/topic-11.md))
- **response time** ([T10](notes/topic-10.md))
- **reusable/consumable resource** ([T11](notes/topic-11.md))
- **root file system** ([T4](notes/topic-04.md))
- **Round-Robin** ([T10](notes/topic-10.md))
- **running/ready/blocked/new/exit** ([T3](notes/topic-03.md))

## S

- **safe/unsafe state** ([T11](notes/topic-11.md))
- **SEM_UNDO**: undo a process's semaphore operations when it exits. ([T7](notes/topic-07.md))
- **semaphore** ([T11](notes/topic-11.md))
- **Semaphore, P/V (wait/signal), binary semaphore**: atomic counter operations for mutual exclusion and synchronisation. ([T7](notes/topic-07.md))
- **serial processing** ([T2](notes/topic-02.md))
- **session** ([T4](notes/topic-04.md))
- **setpgid** ([T4](notes/topic-04.md))
- **setsid** ([T4](notes/topic-04.md))
- **Shared memory (shmget, shmat, shmdt, shmctl)**: one physical segment mapped into several address spaces. ([T7](notes/topic-07.md))
- **SIG_DFL / SIG_IGN / SIG_ERR**: default action / ignore / error return. ([T6](notes/topic-06.md))
- **Signal handler**: void function taking the signal number. ([T6](notes/topic-06.md))
- **Signal mask**: set of currently blocked signals. ([T6](notes/topic-06.md))
- **Signal table**: per-process kernel table of handler addresses. ([T6](notes/topic-06.md))
- **SIGPIPE**: signal sent when writing to a pipe with no reader. ([T7](notes/topic-07.md))
- **sigset_t, struct sigaction**: signal set type and handler description. ([T6](notes/topic-06.md))
- **SISD/SIMD/MISD/MIMD** ([T11](notes/topic-11.md))
- **Slow system call / EINTR**: call such as `accept` or `read` interrupted by a signal. ([T8](notes/topic-08.md))
- **SMP** ([T2](notes/topic-02.md), [T11](notes/topic-11.md))
- **Socket**: communication endpoint identified by IP + port. ([T8](notes/topic-08.md))
- **spawn** ([T3](notes/topic-03.md))
- **SPN** ([T10](notes/topic-10.md))
- **SRT** ([T10](notes/topic-10.md))
- **starvation** ([T10](notes/topic-10.md), [T11](notes/topic-11.md))
- **super block** ([T5](notes/topic-05.md))
- **supervisor call** ([T3](notes/topic-03.md))
- **suspend** ([T3](notes/topic-03.md))
- **swapping** ([T3](notes/topic-03.md))
- **symbolic link** ([T5](notes/topic-05.md))
- **sync/fsync** ([T5](notes/topic-05.md))
- **System V IPC**: message queue, semaphore set, shared memory. ([T7](notes/topic-07.md))

## T

- **termination status** ([T4](notes/topic-04.md))
- **test-and-set** ([T11](notes/topic-11.md))
- **Thrashing**: excessive swapping. ([T9](notes/topic-09.md))
- **thread** ([T2](notes/topic-02.md), [T11](notes/topic-11.md))
- **throughput** ([T10](notes/topic-10.md))
- **time quantum / time slicing** ([T10](notes/topic-10.md))
- **time sharing** ([T2](notes/topic-02.md))
- **time slice** ([T3](notes/topic-03.md))
- **TLB**: cache of page table entries. ([T9](notes/topic-09.md))
- **trace** ([T3](notes/topic-03.md))
- **trap** ([T3](notes/topic-03.md))
- **turnaround time** ([T10](notes/topic-10.md))

## U

- **umask** ([T4](notes/topic-04.md), [T5](notes/topic-05.md))
- **uniprogramming** ([T2](notes/topic-02.md))
- **Unix domain socket**: same-host IPC addressed by a file name. ([T8](notes/topic-08.md))
- **user mode** ([T2](notes/topic-02.md), [T3](notes/topic-03.md))
- **user-level thread** ([T11](notes/topic-11.md))

## V

- **V-node Table** ([T5](notes/topic-05.md))
- **virtual memory** ([T2](notes/topic-02.md))
- **virtual/real address** ([T2](notes/topic-02.md))

## W

- **wait** ([T4](notes/topic-04.md))
- **waitpid** ([T4](notes/topic-04.md))
- **Well-known port**: 1-1023, assigned by IANA, root only to listen. ([T8](notes/topic-08.md))
- **WEXITSTATUS** ([T4](notes/topic-04.md))
- **WIFEXITED** ([T4](notes/topic-04.md))
- **WIFSIGNALED** ([T4](notes/topic-04.md))
- **WNOHANG** ([T4](notes/topic-04.md))
- **Word size**: size of a CPU register; matches the data bus width. ([T1](notes/topic-01.md))
- **WTERMSIG** ([T4](notes/topic-04.md))

## Z

- **zombie**: terminated child not yet reaped; avoided with a SIGCHLD handler and `waitpid(..., WNOHANG)`. ([T3](notes/topic-03.md), [T4](notes/topic-04.md), [T8](notes/topic-08.md))
