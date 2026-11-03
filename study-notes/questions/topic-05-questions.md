# Topic 05: Unix File Systems and Programming - Questions

[Back to summary](../notes/topic-05.md)

## Lecture slides
### Q1. Any read/write operation for a file must use its file descriptor (not the file name or path, why?) to refer to the file.
Source: Slide 4

### Q2. In the diagram, there are two processes, P1 and P2, each having its own Per Process Table of Open Files, sharing two files: File A and File B. File A is shared via two different File Table entries while File B is shared via the same File Table entry. What are the consequences due to the differences in the way the files are shared? (Refers to the diagram on slide 7.)
Source: Slide 7

### Q3. Example: Copy file fd1 to file fd2

```c
void copy (int fd1, int fd2)
{
       ssize_t nread;
       char buf[100];
       while ((nread=read(fd1,buf,100))>0)
              write(fd2,buf,nread);
}
```
Is the above program correct? If not, what could be the problem? (Hints: think about the write call in the program).
Source: Slide 11

### Q4. The normal output of a command such as ls usually goes to the terminal output. We can redirect this output to a file from the shell prompt: `% ls > foo`. Since no one has changed the program ls in anyway, we must assume that the process ls knows nothing about the redirection. How is this achieved without the knowledge of the process?
Source: Slide 15

### Q5. How does a shell achieve standard I/O redirection in the command below? `% ls > FOO`
Source: Slide 18

### Q6. Suppose that the directory file testdir has the following access permissions (in the form of ls -l output): `drwxr-xr--`. Assuming the files a.out and test.c are stored under the directory testdir, determine whether or not the following types of users are allowed to perform the operations listed below (explain why):
- owner: `% rm testdir/a.out`
- group: `% vi testdir/test.c`, `% rm testdir/a.out`
- others: `% ls testdir`, `% ls -l testdir`, `% vi testdir/test.c`

Source: Slide 27

### Q7. How do we add or remove a file name in a Unix program?
Source: Slide 30

## Textbook
### Q8. When reading or writing a disk file, are the functions described in this chapter really unbuffered? Explain.
Source: APUE Ch 3, Exercise 3.1, p. 90

### Q9. Write your own dup2 function that behaves the same way as the dup2 function described in Section 3.12, without calling the fcntl function. Be sure to handle errors correctly.
Source: APUE Ch 3, Exercise 3.2, p. 90

### Q10. Assume that a process executes the following three function calls:
```c
fd1 = open(path, oflags);
fd2 = dup(fd1);
fd3 = open(path, oflags);
```
Draw the resulting picture, similar to Figure 3.9. Which descriptors are affected by an fcntl on fd1 with a command of F_SETFD? Which descriptors are affected by an fcntl on fd1 with a command of F_SETFL?

Source: APUE Ch 3, Exercise 3.3, p. 90 (Figure 3.9 is in Section 3.10)

### Q11. The following sequence of code has been observed in various programs:
```c
dup2(fd, 0);
dup2(fd, 1);
dup2(fd, 2);
if (fd > 2)
        close(fd);
```
To see why the if test is needed, assume that fd is 1 and draw a picture of what happens to the three descriptor entries and the corresponding file table entry with each call to dup2. Then assume that fd is 3 and draw the same picture.

Source: APUE Ch 3, Exercise 3.4, p. 90

### Q12. The Bourne shell, Bourne-again shell, and Korn shell notation `digit1 >& digit2` says to redirect descriptor digit1 to the same file as descriptor digit2. What is the difference between the two commands shown below? (Hint: The shells process their command lines from left to right.)
```
./a.out > outfile 2>&1
./a.out 2>&1 > outfile
```
Source: APUE Ch 3, Exercise 3.5, p. 90

### Q13. If you open a file for read-write with the append flag, can you still read from anywhere in the file using lseek? Can you use lseek to replace existing data in the file? Write a program to verify this.
Source: APUE Ch 3, Exercise 3.6, p. 91

### Q14. Modify the program in Figure 4.3 to use stat instead of lstat. What changes if one of the command-line arguments is a symbolic link?
Source: APUE Ch 4, Exercise 4.1, p. 141 (Figure 4.3 is the file-type program; the slide 34-35 program is adapted from it)

### Q15. What happens if the file mode creation mask is set to 777 (octal)? Verify the results using your shell's umask command.
Source: APUE Ch 4, Exercise 4.2, p. 141

### Q16. Verify that turning off user-read permission for a file that you own denies your access to the file.
Source: APUE Ch 4, Exercise 4.3, p. 141

## Sample exam
### Q17. What information is stored in the system-wide File Table in a classic Unix system?
a. I-node information, file length, file status flags
b. I-node information, V-node information, current file size
c. File status flags, current file offset, v-node pointer
d. None of the above

Source: Sample exam, Part I, Q 9

### Q18. It is well known that when reading and writing a disk file, the size of the buffer used in the program can affect I/O performance. How would you select the buffer size in order to improve I/O efficiency?
a. Use a buffer whose size is a multiple of 1000
b. Use a buffer whose size is a multiple of 1024
c. Use a buffer whose size is a multiple of the disk block size
d. Use a buffer whose size is as big as possible

Source: Sample exam, Part I, Q 10

### Q19. Assuming a directory contains two regular files named "abc" and "xyz", what would be the content of that directory file?
a. The contents of the files "abc" and "xyz"
b. The file names "abc" and "xyz"
c. The file name and i-node number mapping for files "abc" and "xyz"
d. The file name and i-node number mapping for files "abc" and "xyz" as well as the i-node numbers of the directory itself and its parent directory

Source: Sample exam, Part I, Q 11

### Q20. (8 marks) Write a C program fragment to demonstrate how the standard input of a process may be redirected away from the terminal keyboard to a file.
Source: Sample exam, Part II, Q 3
