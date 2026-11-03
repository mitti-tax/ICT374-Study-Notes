# Topic 06: Signal Handling in UNIX - Questions

[Back to summary](../notes/topic-06.md)

## Lecture slides
The slides contain worked examples but no explicit exercise questions.

## Textbook
### Q1. In Figure 10.2, remove the for (;;) statement. What happens and why?
Figure 10.2 (APUE p. 324) catches SIGUSR1 and SIGUSR2 with the handler `sig_usr`, then runs `for ( ; ; ) pause();` in main.
Source: APUE Ch 10, Exercise 10.1, p. 381

### Q2. Write a program that calls fwrite with a large buffer (about one gigabyte). Before calling fwrite, call alarm to schedule a signal in 1 second. In your signal handler, print that the signal was caught and return. Does the call to fwrite complete? What's happening?
Source: APUE Ch 10, Exercise 10.12, p. 382

## Sample exam
### Q3. Which signal would be sent to the foreground processes by pressing Ctrl-C on the keyboard?
a. SIGKILL
b. SIGCHILD
c. SIGQUIT
d. SIGINT

Source: Sample exam, Q 12 (Part I)

### Q4. What does the following system call do?
`pause();`

a. Pause the calling process for 1 second
b. Suspend the calling process until a signal is received and processed.
c. Pause the calling until SIGALRM is received
d. Pause the calling process until its child process catches up

Source: Sample exam, Q 13 (Part I)

### Q5. What is the effect of the following function call to the calling process?
`signal(SIGINT, SIG_IGN);`

a. Send signal SIGINT to the foreground processes
b. Install signal handler for SIGINT
c. Pressing Ctrl-\ will kill the calling process
d. Pressing Ctrl-C will not kill the calling process

Source: Sample exam, Q 14 (Part I)

### Q6. In the reliable signal model, a signal is "pending" means?
a. The signal is not yet generated
b. The signal is generated but not yet processed
c. The signal is ignored
d. The signal is caught

Source: Sample exam, Q 15 (Part I)

### Q7. Which system call would you use to unblock certain signals?
a. sigaction
b. sigprocmask
c. sigpending
d. signal

Source: Sample exam, Q 16 (Part I)

### Q8. (8 marks) Read the following program. Assuming that you run the program on the foreground from a terminal, answer the following questions:
a) What would happen if you type Ctrl-C on the terminal, and why?
b) What would happen if you type Ctrl-\ on the terminal, and why?
c) What would happen if you type Ctrl-C three times in quick succession on the terminal, and why?

```c
#include <signal.h>
#include <stdio.h>

void catch(int signo)
{
       printf("Signal %d caught\n", signo);
       sleep (2);
}

int main()
{
       int i;
       struct sigaction act;

       act.sa_flags = 0;
       act.sa_handler = catch;
       sigfillset( & (act.sa_mask) );

       if (sigaction(SIGINT, &act, NULL) != 0) {
                  perror("sigaction"); exit(1);
       }

       for (i=0; i<100; ++i) {
              printf("sleep %d ....\n", i); sleep (1);
       }

       exit(0);
}
```
Source: Sample exam, Q 5 (Part II)
