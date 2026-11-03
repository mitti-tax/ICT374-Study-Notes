# Topic 07: Inter-Process Communications Mechanisms in Unix - Questions

[Back to summary](../notes/topic-07.md)

## Lecture slides
### Q1. Refer to the line labels in the program below (pipesync.c, slides 8-9). Explain what would happen if Line L1: "close(BA[1])" is deleted. When analysing the behaviour of the program, remember the behaviours of the read and write system calls when a pipe is empty or full, as detailed at the beginning of the section.

```c
/* pipesync.c - demonstrating synchronisation
 * and deadlock when using pipes
*/
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
char *msg="Hello, there!";
int BUFSIZE = 100;
// convert upper case to lower case or vice versa
// (simulating some kind of service)
void conv(char *s, int len)
{
     char c;
     int i;
     for(i=0; i<len; ++i){
          c = s[i];
          if (islower(c))
                s[i]=toupper(c);
          else if (isupper(c))
                s[i]=tolower(c);
     }
}
int main()
{
     pid_t pid;
     int AB[2], BA[2];
     char buf[BUFSIZE];
     int i, n;
// create two pipes
if (pipe(AB)<0 || pipe(BA)<0)
     perror("pipe calls"), exit(1);
// create process B
if((pid=fork())<0)
     perror("fork call"), exit(2);
if (pid>0) {        /* process A */
     close(AB[0]);                     // L1
     close(BA[1]);
     printf(" A sends: %s\n", msg);
     write(AB[1], msg, strlen(msg));
     close(AB[1]);                     // L2
     printf("A receives: ");
     while((n=read(BA[0], buf, BUFSIZE))>0)
          buf[n]='\0', printf("%s", buf);
     printf("\n"); exit(0);
}
if (pid==0) { /* porcess B */
     close(BA[0]);
     close(AB[1]);                     // L3
     while((n=read(AB[0], buf, BUFSIZE))>0){
           conv(buf, n);
           write(BA[1], buf, n);
     }
     exit(0);
}
}
```
(The slide diagram shows two pipes between A and B: AB, written at AB[1] and read at AB[0], and BA, written at BA[1] and read at BA[0].)

Source: Slide 10 (program on Slides 8-9)

### Q2. Refer to the line labels in the pipesync.c program (Q1). Explain what would happen if Line L2: "close(AB[1])" is deleted.

Source: Slide 10

### Q3. Refer to the line labels in the pipesync.c program (Q1). Explain what would happen if Line L3: "close(AB[1])" is deleted.

Source: Slide 10

### Q4. (In rfile.c, after the reader has finished its loop) "tell wfile that I have completed, so it can remove the semaphores. Note we must use the none undo version of the v to signal (think why this is necessary)": `v0(end);`

Source: Slide 39

## Textbook
### Q5. What operations can be performed on a semaphore?

Source: Stallings Ch 5, Review Question 5.8, p. 276

### Q6. What is the difference between binary and general semaphores?

Source: Stallings Ch 5, Review Question 5.9, p. 276

### Q7. Consider the following definition of semaphores:

```
void semWait(s)
{
   if (s.count > 0){
     s.count--;
}
   else {
     place this process in s.queue;
    block;
   }
}
void semSignal (s)
{
   if (there is at least one process blocked on
     semaphore s) {
      remove a process P from s.queue;
      place process P on ready list;
   }
   else      s.count++;
}
```
Compare this set of definitions with that of Figure 5.6. Note one difference: With the preceding definition, a semaphore can never take on a negative value. Is there any difference in the effect of the two sets of definitions when used in programs? That is, could you substitute one set for the other without altering the meaning of the program?

Source: Stallings Ch 5, Problem 5.15, p. 281 (refers to Figure 5.6)

### Q8. Show that message passing and semaphores have equivalent functionality by
a. Implementing message passing using semaphores. Hint: Make use of a shared buffer area to hold mailboxes, each one consisting of an array of message slots.
b. Implementing a semaphore using message passing. Hint: Introduce a separate synchronization process.

Source: Stallings Ch 5, Problem 5.27, p. 287

### Q9. What happens if a malicious process reads a message from a message queue that is being used by a server and several clients? What information does the malicious process need to know to read the message queue? (Preamble in the exercise: "Unless a file contains sensitive or confidential data, allowing other users to read the file causes no harm. (It is usually considered antisocial, however, to go snooping around in other people's files.) But what happens if ...")

Source: APUE Ch 15, Exercise 15.11, p. 588

### Q10. Write a program that does the following. Execute a loop five times: create a message queue, print the queue identifier, delete the message queue. Then execute the next loop five times: create a message queue with a key of IPC_PRIVATE, and place a message on the queue. After the program terminates, look at the message queues using ipcs(1). Explain what is happening with the queue identifiers.

Source: APUE Ch 15, Exercise 15.12, p. 588

### Q11. Describe how to build a linked list of data objects in a shared memory segment. What would you store as the list pointers?

Source: APUE Ch 15, Exercise 15.13, p. 588

### Q12. Draw a timeline of the program in Figure 15.33 showing the value of the variable i in both the parent and child, the value of the long integer in the shared memory region, and the value returned by the update function. Assume that the child runs first after the fork.

Source: APUE Ch 15, Exercise 15.14, p. 588 (refers to Figure 15.33, p. 577: parent/child IPC using memory-mapped /dev/zero)

### Q13. Redo the program in Figure 15.33 using the XSI shared memory functions from Section 15.9 instead of the shared memory-mapped region.

Source: APUE Ch 15, Exercise 15.15, p. 588 (refers to Figure 15.33, p. 577)

### Q14. Redo the program in Figure 15.33 using the XSI semaphore functions from Section 15.8 to alternate between the parent and the child.

Source: APUE Ch 15, Exercise 15.16, p. 588 (refers to Figure 15.33, p. 577)

## Sample exam
### Q15. In Unix programming, mutual exclusion can be implemented by using
a. Multiple threads
b. A binary semaphore
c. Signals
d. All of the above

Source: Sample exam, Part I, Q17

### Q16. In operating systems, the Producer-Consumer Problem is used to
a. Produce items for consumption
b. Illustrate and test the synchronization of multiple processes
c. Implement semaphores
d. Generate random numbers

Source: Sample exam, Part I, Q18

### Q17. (8 marks) Write a UNIX program to demonstrate how a child process passes a message to its parent process using an unnamed pipe.

Source: Sample exam, Part II, Q4
