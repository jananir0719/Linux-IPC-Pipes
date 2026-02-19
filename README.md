# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls

<img width="687" height="834" alt="Screenshot from 2026-02-14 19-55-14" src="https://github.com/user-attachments/assets/4bebc18d-ee6b-4fa2-ba2c-05abdf1242eb" />




## OUTPUT
<img width="538" height="123" alt="Screenshot from 2026-02-14 19-54-40" src="https://github.com/user-attachments/assets/3fc6c849-8a98-4d6f-829a-f6d83cdb20b3" />


## C Program that illustrate communication between two process using named pipes using Linux API system calls

#include <stdio.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int fd;
    // Path for the named pipe
    char *myfifo = "/tmp/myfifo_janani";

    // Create the named pipe (FIFO)
    mkfifo(myfifo, 0666);

    pid_t pid = fork();

    if (pid < 0) {
        printf("Fork Failed\n");
        return 1;
    }

    // CHILD PROCESS: The Reader
    if (pid == 0) {
        char str_rx[80];
        // Open pipe for reading
        fd = open(myfifo, O_RDONLY);
        read(fd, str_rx, 80);
        printf("\n[Child Process] Message Received: %s\n", str_rx);
        close(fd);
    } 
    // PARENT PROCESS: The Writer
    else {
        char str_tx[80] = "Hello Janani! Communication via Named Pipe is successful.";
        printf("[Parent Process] Sending data to child...\n");
        
        // Open pipe for writing
        fd = open(myfifo, O_WRONLY);
        write(fd, str_tx, strlen(str_tx) + 1);
        close(fd);
        
        // Wait for child to finish printing before ending
        wait(NULL); 
        // Remove the pipe file from the system
        unlink(myfifo); 
    }

    return 0;
}




## OUTPUT
<img width="918" height="270" alt="Screenshot from 2026-02-19 08-42-24" src="https://github.com/user-attachments/assets/6fbc9ac3-146b-4bba-9e3a-3e17f5708055" />


# RESULT:
The program is executed successfully.
