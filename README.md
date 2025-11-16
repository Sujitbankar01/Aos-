slip 2
Q1. Write a C program to find file properties such as inode number, number of hard link, File 
permissions, File size, File access and modification time and so on of a given file using stat() 
system call.
#include <stdio.h>

#include <stdlib.h>

#include <sys/stat.h>

#include <unistd.h>

#include <errno.h>

#include <time.h>

int main(int argc, char *argv[]) {

 if (argc != 2) {
 
Downloaded by Aritc369 (artic1899@gmail.com) printf("Usage: %s <filename>\n", argv[0]);

 return 1;
 
 }
 
 struct stat fileStat;
 

 if (stat(argv[1], &fileStat) < 0) {

 fprintf(stderr, "Error accessing file %s: %s\n", argv[1], strerror(errno));

 return 1;

 }

 printf("File: %s\n", argv[1]);

 printf("Inode Number: %ld\n", (long)fileStat.st_ino);

 printf("Number of Hard Links: %ld\n", (long)fileStat.st_nlink);
 
 printf("File Permissions: ");
 
 printf((S_ISDIR(fileStat.st_mode)) ? "d" : "-");

 printf((fileStat.st_mode & S_IRUSR) ? "r" : "-");

 printf((fileStat.st_mode & S_IWUSR) ? "w" : "-");


 printf((fileStat.st_mode & S_IXUSR) ? "x" : "-");

 printf((fileStat.st_mode & S_IRGRP) ? "r" : "-");


 printf((fileStat.st_mode & S_IWGRP) ? "w" : "-");

 printf((fileStat.st_mode & S_IXGRP) ? "x" : "-");

 printf((fileStat.st_mode & S_IROTH) ? "r" : "-");

 printf((fileStat.st_mode & S_IWOTH) ? "w" : "-");

 printf((fileStat.st_mode & S_IXOTH) ? "x\n" : "-\n");

 printf("File Size: %ld bytes\n", (long)fileStat.st_size);

 printf("Last Access Time: %s", ctime(&fileStat.st_atime));

 printf("Last Modification Time: %s", ctime(&fileStat.st_mtime));

 return 0;
 }

Output:

File: example.txt

Inode Number: 123456

Number of Hard Links: 1

File Permissions: -rw-r--r--

File Size: 1024 bytes


Last Access Time: Mon Jan 10 15:30:00 2023

Last Modification Time: Fri Dec 15 09:45:00 2023

Q2. Write a C program that catches the ctrl-c (SIGINT) signal for the first time and display 
the appropriate message and exits on pressing ctrl-c again.

#include <stdio.h>

#include <signal.h>
#include <stdlib.h>

int count = 0;

void handle_sigint(int signum) {

 if (signum == SIGINT) {

 if (count == 0) {

 printf("\nCaught SIGINT signal. Press Ctrl+C again to exit.\n");

count++;

 } else {


 printf("\nReceived second SIGINT. Exiting...\n");


 int main() {

 signal(SIGINT, handle_sigint);

 printf("Press Ctrl+C to trigger SIGINT signal.\n");
 while(1) {

 // Program continues running and waiting for SIGINT
 }

 return 0;
}exit(EXIT_SUCCESS);
 }
 }
}
Q.2) Write a C program to create an unnamed pipe. The child process will write following 
three messages to pipe and parent process display it. 

Message1 = “Hello World” 

Message2 = “Hello SPPU”


Message3 = “Linux is Funny”

#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>

#include <string.h>

#define MSG_SIZE 100

int main() {

 int pipefd[2];

 pid_t pid;

 char message[3][MSG_SIZE] = {

 "Hello World",

 "Hello SPPU",

 "Linux is Funny"

 };

 if (pipe(pipefd) == -1) {

 perror("Pipe creation failed");

 exit(EXIT_FAILURE);

 }

 pid = fork();

 if (pid < 0) {

 perror("Fork failed");

 exit(EXIT_FAILURE);

 } else if (pid == 0) { // Child process


 close(pipefd[0]); // Close reading end of pipe in child

 for (int i = 0; i < 3; i++) {

 write(pipefd[1], message[i], 
 
 strlen(message[i]) + 1);
 }

 close(pipefd[1]); // Close writing end of pipe in child

 } else { // Parent process

 close(pipefd[1]); // Close writing end of 
 
 pipe in parent

 char buffer[MSG_SIZE];

 while (read(pipefd[0], buffer, MSG_SIZE) > 0) {

 printf("Message received: %s\n", buffer);

 }

 close(pipefd[0]); // Close reading end of 
 pipe in parent

 }

 return 0;

}

Output :
Message received: Hello World

Message received: Hello SPPU
Message received: Linux is Funny
