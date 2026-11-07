# Unix programming cheat sheet (Topics 4-8)

Quick reference built from the lecture slides. Details and caveats are in the topic notes: [4](notes/topic-04.md), [5](notes/topic-05.md), [6](notes/topic-06.md), [7](notes/topic-07.md), [8](notes/topic-08.md). Every call returns -1 on error unless noted.

`#include`s come from the prototype boxes in APUE 3rd ed. (`gethostbyname`, `inet_addr`/`inet_ntoa` and `bzero`/`bcopy` aren't boxed in APUE, so those are from the Linux headers). The lecture code also includes `<sys/types.h>` in most programs, and `<sys/ipc.h>` for System V IPC. Neither hurts, so add them if you want to match the slides.

A header block that covers everything on this sheet:

```c
#include <stdio.h>        /* printf, perror, rename */
#include <stdlib.h>       /* exit, getenv */
#include <string.h>       /* strlen, strcpy, memset */
#include <strings.h>      /* bzero, bcopy, bcmp */
#include <errno.h>        /* errno, EINTR, EAGAIN */
#include <unistd.h>       /* fork, exec*, read, write, close, dup, pipe, alarm, pause */
#include <fcntl.h>        /* open, fcntl, O_* flags */
#include <sys/types.h>    /* pid_t, off_t, ssize_t (lecture code includes it) */
#include <sys/stat.h>     /* stat, chmod, umask, mkfifo, S_* macros */
#include <sys/wait.h>     /* wait, waitpid, W* macros */
#include <dirent.h>       /* opendir, readdir, closedir */
#include <signal.h>       /* kill, signal, sigaction, sigset functions */
#include <sys/ipc.h>      /* IPC_CREAT, IPC_PRIVATE, IPC_RMID */
#include <sys/msg.h>      /* msgget, msgsnd, msgrcv, msgctl */
#include <sys/sem.h>      /* semget, semop, semctl, struct sembuf */
#include <sys/shm.h>      /* shmget, shmat, shmdt, shmctl */
#include <sys/socket.h>   /* socket, bind, listen, accept, connect, send/recv */
#include <netinet/in.h>   /* struct sockaddr_in, INADDR_ANY */
#include <sys/un.h>       /* struct sockaddr_un */
#include <arpa/inet.h>    /* htons/htonl/ntohs/ntohl, inet_addr, inet_ntoa */
#include <netdb.h>        /* gethostbyname, getservbyname */
```

## Processes (Topic 4)

| Call | `#include` | Meaning |
|---|---|---|
| `pid_t fork(void)` | `<unistd.h>` | duplicate caller. Returns 0 in child, child PID in parent, -1 on error |
| `execl(path, arg0, ..., (char*)0)` / `execv(path, argv)` | `<unistd.h>` | replace program; `path` is a pathname |
| `execlp(file, arg0, ...)` / `execvp(file, argv)` | `<unistd.h>` | same, but search `PATH` |
| `execle(path, arg0, ..., (char*)0, envp)` / `execve(path, argv, envp)` | `<unistd.h>` | same, with a new environment |
| `void exit(int status)` | `<stdlib.h>` | close fds, free memory, end process, status to parent |
| `pid_t wait(int *stat)` | `<sys/wait.h>` | block until any child ends; returns its PID |
| `pid_t waitpid(pid, &stat, opt)` | `<sys/wait.h>` | `pid`: -1 any, >0 that child, 0 same group, <-1 group `\|pid\|`. `opt`: 0 block, `WNOHANG` don't |
| `WIFEXITED(s)` `WEXITSTATUS(s)` `WIFSIGNALED(s)` `WTERMSIG(s)` | `<sys/wait.h>` | decode status |
| `setpgid(pid, pgid)` / `setsid()` | `<unistd.h>` | move to a process group / new session with no terminal |
| `getenv("USER")`, `extern char **environ`, `main(argc, argv, envp)` | `<stdlib.h>` (`getenv`) | read the environment |

A successful `exec` never returns, so code after it runs only on failure.

```c
pid = fork();
if (pid < 0)       { perror("fork"); exit(1); }
else if (pid == 0) { execl("/bin/ls", "ls", "-l", (char *)0);
                     perror("execl"); exit(1); }   /* only if exec failed */
else               { wait(&status); }
```

```c
void daemon_init(void) {            /* slide 26 */
    if (fork() != 0) exit(0);       /* parent exits, shell regains control */
    setsid();                       /* new session, no control terminal */
    chdir("/");                     /* allow unmounting */
    umask(0);                       /* exact permissions on new files */
}
```

## Files (Topic 5)

| Call | `#include` | Meaning |
|---|---|---|
| `open(path, oflag, mode)` | `<fcntl.h>` (+ `<sys/stat.h>` for mode bits) | lowest free fd. `O_RDONLY/O_WRONLY/O_RDWR` + `O_APPEND`, `O_CREAT`, `O_EXCL`, `O_TRUNC`, `O_NONBLOCK` |
| `close(fd)` | `<unistd.h>` | 0 or -1 |
| `ssize_t read(fd, buf, n)` | `<unistd.h>` | bytes read; 0 = EOF |
| `ssize_t write(fd, buf, n)` | `<unistd.h>` | bytes written (can be fewer than `n`) |
| `off_t lseek(fd, off, whence)` | `<unistd.h>` | `SEEK_SET`, `SEEK_CUR`, `SEEK_END`; no I/O; returns new offset |
| `dup(fd)` / `dup2(fd, fd2)` | `<unistd.h>` | copy to lowest free fd / to `fd2` (closing it first); share one File Table entry |
| `fcntl(fd, cmd, arg)` | `<fcntl.h>` | `F_DUPFD`, `F_GETFD/F_SETFD` (`FD_CLOEXEC`), `F_GETFL/F_SETFL` (status flags) |
| `stat/fstat/lstat(.., &sb)` | `<sys/stat.h>` | fill `struct stat`; `lstat` doesn't follow symlinks |
| `link`, `symlink`, `readlink`, `unlink`, `rename` | `<unistd.h>`; `rename`: `<stdio.h>` | names: hard link, symbolic link, read link text, remove name, rename |
| `chmod(path, mode)`, `fchmod(fd, mode)`, `umask(mask)` | `<sys/stat.h>` | permissions; mask clears bits on new files |
| `sync()` / `fsync(fd)` | `<unistd.h>` | schedule flush of all / flush one file and wait |
| `opendir`, `readdir`, `rewinddir`, `closedir` | `<dirent.h>` | read a directory (`d_ino`, `d_name`) |

Type macros on `st_mode` (`<sys/stat.h>`): `S_ISREG S_ISDIR S_ISCHR S_ISBLK S_ISFIFO S_ISLNK S_ISSOCK`. Permission bits: `S_IRUSR S_IWUSR S_IXUSR S_IRGRP ... S_IXOTH`.

Kernel tables: per-process fd table (fd flags) -> **File Table** (one per `open`: status flags, offset) -> **V-node table** (one per file: type, i-node). `fork` and `dup` share a File Table entry (shared offset); two `open`s do not.

```c
/* shell's "ls > FOO" */
fd = open("FOO", O_WRONLY|O_CREAT|O_TRUNC, 0644);
if (fork() == 0) {
    dup2(fd, STDOUT_FILENO);        /* = close(1); dup(fd); */
    close(fd);
    execlp("ls", "ls", (char *)0);
}
```

```c
/* make a file with exactly 0766 */
old = umask(0);  open("foo", O_RDWR|O_CREAT, 0766);  umask(old);
/* set O_NONBLOCK */
val = fcntl(fd, F_GETFL, 0);  fcntl(fd, F_SETFL, val | O_NONBLOCK);
```

Permission check: owner bits if effective uid owns the file; else group bits if effective gid matches; else other bits (only one set is used). Directory: r = list, w = add/remove entries, x = search / `cd`.

## Signals (Topic 6)

Key numbers (slide 4; vary between systems): SIGHUP 1, SIGINT 2 (Ctrl-C), SIGQUIT 3 (Ctrl-\), SIGKILL 9 (can't be caught/ignored), SIGSEGV 11, SIGPIPE 13, SIGALRM 14, SIGTSTP 18 (Ctrl-Z), SIGCHLD 20, SIGUSR1/2 30/31.

| Call | `#include` | Meaning |
|---|---|---|
| `kill(pid, signo)` | `<signal.h>` | send a signal |
| `signal(signo, handler)` | `<signal.h>` | old interface, returns previous handler; `SIG_DFL`, `SIG_IGN`, `SIG_ERR` |
| `alarm(n)` | `<unistd.h>` | SIGALRM in `n` s; returns seconds left on previous alarm; `alarm(0)` cancels |
| `pause()` | `<unistd.h>` | sleep until a handler has run |
| `sigaction(signo, &act, &old)` | `<signal.h>` | install handler (use this) |
| `sigemptyset/sigfillset/sigaddset/sigdelset(&set, ..)`, `sigismember` | `<signal.h>` | build `sigset_t` (`sigismember`: 1/0) |
| `sigprocmask(how, &set, &old)` | `<signal.h>` | `SIG_BLOCK` add, `SIG_UNBLOCK` remove, `SIG_SETMASK` replace |
| `sigpending(&set)` | `<signal.h>` | signals blocked and waiting |

```c
struct sigaction act;
act.sa_handler = catch;             /* void catch(int signo) */
sigemptyset(&act.sa_mask);          /* extra signals blocked during handler */
act.sa_flags = 0;                   /* SA_NOCLDSTOP for SIGCHLD, SA_RESETHAND, SA_SIGINFO */
sigaction(SIGINT, &act, NULL);
```

- Same-number signals are blocked while their handler runs. POSIX does **not** queue: several blocked occurrences are delivered once.
- Life of a signal: generated -> pending -> delivered. Blocked signals stay pending.
- Slow calls interrupted by a signal fail with `errno == EINTR`: check and retry.

```c
/* critical section */
sigemptyset(&set);  sigaddset(&set, SIGINT);  sigaddset(&set, SIGQUIT);
sigprocmask(SIG_BLOCK, &set, &old);
/* ... work ... */
sigprocmask(SIG_SETMASK, &old, NULL);
```

Reaping children (Topic 8): handler for SIGCHLD does `while (waitpid(0, NULL, WNOHANG) > 0);`.

## IPC (Topic 7)

**Pipes and FIFOs**

- `pipe(fd)` (`<unistd.h>`): `fd[0]` read, `fd[1]` write. Create **before** `fork`; each side closes the end it doesn't use.
- Empty + writer connected: `read` blocks. Empty + no writer: `read` returns 0. Full: `write` blocks. No reader: SIGPIPE (if caught/ignored, `write` returns -1).
- Non-blocking: `O_NONBLOCK` via `fcntl`; then empty read / full write gives -1 with `EAGAIN`.
- `mkfifo(path, mode)` (`<sys/stat.h>`): a named pipe for unrelated processes; then `open/read/write`.

**System V IPC** (`IPC_*` constants in `<sys/ipc.h>`; key -> id; persists until removed; `ipcs` lists, `ipcrm` removes; `IPC_PRIVATE` for related processes)

| Object | `#include` | Calls |
|---|---|---|
| Message queue | `<sys/msg.h>` | `msgget(key, 0600\|IPC_CREAT)`, `msgsnd(id, &msg, size, flag)`, `msgrcv(id, &msg, size, type, flag)`, `msgctl(id, IPC_RMID, 0)` |
| Semaphore set | `<sys/sem.h>` | `semget(key, n, flags)` (no initial value), `semctl(id, num, SETVAL / IPC_RMID, arg)`, `semop(id, ops, nops)` |
| Shared memory | `<sys/shm.h>` | `shmget(key, size, flags)`, `shmat(id, NULL, 0)`, `shmdt(addr)`, `shmctl(id, IPC_RMID, 0)` |

- Message: `struct { long mtype; char mtext[N]; }`, `mtype > 0`, `size` = length of `mtext`.
- `msgrcv` type: `0` first message; `>0` first with that type; `<0` lowest type <= `|type|`. `IPC_NOWAIT` -> `EAGAIN`/`ENOMSG` instead of blocking; `MSG_NOERROR` truncates a long message (else `E2BIG`). `IPC_EXCL` + `IPC_CREAT` fails with `EEXIST`.
- `struct sembuf {sem_num; sem_op; sem_flg}`: `sem_op < 0` = P (wait), `> 0` = V (signal), `0` = wait for zero. `SEM_UNDO` reverses the op when the process exits, so use the non-undo version when the V must outlive the process.

```c
P(sem); /* critical section */ V(sem);          /* mutual exclusion, initial value 1 */

/* producer-consumer, one-item buffer: empty = 1, full = 0 */
producer:  produce; p(empty); put item; v(full);
consumer:  p(full);  get item; v(empty); consume;
```

## Sockets (Topic 8)

| Call | `#include` | Meaning |
|---|---|---|
| `socket(family, type, 0)` | `<sys/socket.h>` | `AF_UNIX`/`AF_INET`; `SOCK_STREAM` (TCP), `SOCK_DGRAM` (UDP) |
| `bind(sd, addr, len)` | `<sys/socket.h>` | local address. Required for servers |
| `listen(sd, backlog)` | `<sys/socket.h>` | ready for connections (usually 5) |
| `accept(sd, &peer, &len)` | `<sys/socket.h>` | blocks; returns a **new** socket for that client |
| `connect(sd, addr, len)` | `<sys/socket.h>` | TCP: establish connection. UDP: remember destination |
| `read/write`, `send/recv` | `<unistd.h>` (`read/write`), `<sys/socket.h>` (`send/recv`) | data on a connected socket |
| `sendto/recvfrom` | `<sys/socket.h>` | datagrams with explicit peer address |
| `gethostbyname(name)`, `getservbyname("ftp","tcp")` | `<netdb.h>` | host / port lookup (`h_addr`; `s_port` is network order) |
| `htons/htonl`, `ntohs/ntohl` | `<arpa/inet.h>` | host <-> network byte order (16/32-bit) |
| `inet_addr`, `inet_ntoa` | `<arpa/inet.h>` | dotted string <-> 32-bit address |
| `getsockname`, `getpeername` | `<sys/socket.h>` | local / remote address of a socket |

Call order:

- TCP server: `socket, bind, listen, accept, read, process, write, close`. TCP client: `socket, connect, write, read, close`.
- UDP server: `socket, bind`, loop `recvfrom, process, sendto`. UDP client: `socket`, `bind` own address (slide 18; lets the server reply), `sendto`, `recvfrom`.

```c
struct sockaddr_in a;               /* <netinet/in.h> */
bzero(&a, sizeof a);
a.sin_family      = AF_INET;
a.sin_port        = htons(40000);
a.sin_addr.s_addr = htonl(INADDR_ANY);   /* server; client: *(u_long*)hp->h_addr */
```

Unix domain: `struct sockaddr_un` (`<sys/un.h>`), `sun_family = AF_UNIX`, `sun_path` = file name; `unlink` it before `bind`.

Concurrent TCP server loop:

```c
for (;;) {
    ns = accept(s, (struct sockaddr *)&peer, &len);
    if (ns < 0) { if (errno == EINTR) continue; exit(1); }
    if (fork() == 0) { close(s); serve_a_client(ns); exit(0); }
    close(ns);                      /* parent: keep listening */
}
```

Pitfalls from the slides: TCP is a byte stream (one `write` is not one `read`), so frame messages with a 2-byte length in network order (`readn`/`writen`). UDP can lose datagrams: `alarm` timeout, double the interval, resend, number each message to drop stale duplicates. Byte order: a big-endian 20 (`00 14`) reads as 5120 on little-endian. Use `bcopy/bzero/bcmp`, not `str*`, for binary data.
