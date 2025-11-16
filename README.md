# Aos-
Slip 1 to 5
slip1
Q1. Take multiple files as Command Line Arguments and print their inode numbers and file 
types.
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include <errno.h>
int main(int argc, char *argv[]) {
 if (argc < 2) {
 printf("Usage: %s <file1> <file2> ... <fileN>\n", argv[0]);
 return 1;
 }
 for (int i = 1; i < argc; i++) {
 struct stat fileStat;
 if (stat(argv[i], &fileStat) < 0) {
 fprintf(stderr, "Error accessing file %s: %s\n", argv[i], strerror(errno));
 continue;
 }
 printf("File: %s\n", argv[i]);
 printf("Inode Number: %ld\n", (long)fileStat.st_ino);
 if (S_ISREG(fileStat.st_mode)) {
 printf("File Type: Regular file\n");
 } else if (S_ISDIR(fileStat.st_mode)) {
 printf("File Type: Directory\n");
 } else if (S_ISLNK(fileStat.st_mode)) {
 printf("File Type: Symbolic Link\n");
 } else if (S_ISCHR(fileStat.st_mode)) {
 printf("File Type: Character Device\n");
 } else if (S_ISBLK(fileStat.st_mode)) {
 printf("File Type: Block Device\n");
 } else if (S_ISFIFO(fileStat.st_mode)) {
 printf("File Type: FIFO/Named Pipe\n");
 } else if (S_ISSOCK(fileStat.st_mode)) {
 printf("File Type: Socket\n");
 } else {
 printf("File Type: Unknown\n");
 }
 printf("\n");
 }
 return 0;
}
Output:
File: file1.txt
Inode Number: 123456
File Type: Regular file
File: file2.txt
Inode Number: 789012
File Type: Regular file
File: directory
Inode Number: 345678
File Type: Directory
Q2. Write a C program to send SIGALRM signal by child process to parent process and 
parent process make a provision to catch the signal and display alarm is fired.(Use Kill, fork, 
signal and sleep system call).
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <sys/types.h>
void handle_alarm(int signum) {
 if (signum == SIGALRM) {
 printf("Alarm is fired!\n");
 }
}
int main() {
 pid_t pid;
 signal(SIGALRM, handle_alarm); // Registering the signal handler
 pid = fork();
 if (pid < 0) {
 perror("Fork failed");
 exit(EXIT_FAILURE);
Downloade} else if (pid == 0) { // Child process
 sleep(2); // Waiting for 2 seconds
 kill(getppid(), SIGALRM); // Sending SIGALRM signal to the parent
 } else { // Parent process
 printf("Waiting for alarm...\n");
 while(1) {
 // Parent process waits indefinitely for the signal
 // The signal handler will execute upon receiving SIGALRM
 }
 }
 return 0;
}
Output:
Waiting for alarm...
Alarm is fired!
