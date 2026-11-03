# Topic 04: Unix Processes and Programming - Questions

[Back to summary](../notes/topic-04.md)

The lecture slides contain no practice questions.

## Textbook
### Q1. On an Intel x86 system under Linux, if we execute the program that prints "hello, world" and do not call exit or return, the termination status of the program—which we can examine with the shell—is 13. Why?
Source: APUE Ch 7, Exercise 7.1, p. 226

### Q2. In Figure 8.3, we said that replacing the call to _exit with a call to exit might cause the standard output to be closed and printf to return −1. Modify the program to check whether your implementation behaves this way. If it does not, how can you simulate this behavior?
Source: APUE Ch 8, Exercise 8.1, p. 283 (refers to Figure 8.3)

### Q3. Recall the typical arrangement of memory in Figure 7.6. Because the stack frames corresponding to each function call are usually stored in the stack, and because after a vfork the child runs in the address space of the parent, what happens if the call to vfork is from a function other than main and the child does a return from this function after the vfork? Write a test program to verify this, and draw a picture of what's happening.
Source: APUE Ch 8, Exercise 8.2, p. 283 (refers to Figure 7.6)

### Q4. When we execute the program in Figure 8.13 one time, as in

    $ ./a.out

the output is correct. But if we execute the program multiple times, one right after the other, as in

    $ ./a.out ; ./a.out ; ./a.out
    output from parent
    ooutput from parent
    ouotuptut from child
    put from parent
    output from child
    utput from child

the output is not correct. What's happening? How can we correct this? Can this problem happen if we let the child write its output first?

Source: APUE Ch 8, Exercise 8.4, p. 283 (refers to Figure 8.13)

### Q5. Write a program that creates a zombie, and then call system to execute the ps(1) command to verify that the process is a zombie.
Source: APUE Ch 8, Exercise 8.6, p. 283

### Q6. Write a small program that calls fork and has the child create a new session. Verify that the child becomes a process group leader and that the child no longer has a controlling terminal.
Source: APUE Ch 9, Exercise 9.2, p. 312

### Q7. Recall the sample ps output from Section 13.2. The only user-level daemon that isn't a session leader is the rsyslogd process. Explain why the syslogd daemon isn't a session leader.
Source: APUE Ch 13, Exercise 13.2, p. 480

### Q8. Write a program that calls the daemonize function in Figure 13.1. After calling this function, call getlogin (Section 8.15) to see whether the process has a login name now that it has become a daemon. Print the results to a file.
Source: APUE Ch 13, Exercise 13.4, p. 480 (refers to Figure 13.1)

## Sample exam
### Q9. The Process Image element that contains the modifiable part of the user space is called the:
a. User Program
b. System Stack
c. Process Control Block
d. None of the above

Source: Sample exam, Part I Q3

### Q10. Given the following Unix code excerpt:

    execl("/bin/ls", "ls", (char *)0);
    printf("end of ls\n");

What is the most likely output from the above code?
a. Listing of the filenames in the current directory
b. Listing of the filenames in the current directory followed by the line "end of ls"
c. An error message
d. No output will be generated

Source: Sample exam, Part I Q7

### Q11. Which of the following statements about daemon process is false?
a. A daemon process is usually a server process
b. Each daemon process has a controlling terminal
c. A daemon process usually keeps running until the machine is shut down
d. A daemon process usually reports error and diagnostic information by writing a message to a log file

Source: Sample exam, Part I Q8

### Q12. (8 marks) Write a program that
a) prints a list of its environment variables (and their values); and
b) prints out the current directory of the process as well as the user who invokes the program.

Source: Sample exam, Part II Q2
