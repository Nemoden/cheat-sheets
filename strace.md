strace
==

`-o` param saves strace output to the file specified

    strace -o output.txt ls

`-t` prints timestamp

    strace -t -e open ls /home

`-r` prints relative time from start

    strace -r ls

`-p` attaches to a PID

    strace -p <PID>

`-e` specifies syscall(s)

    strace -e open,close ls

`-s` tells how many bytes one line may contain at most

    strace -s 1024 ls

`-c` see what time is spend and where (combine with `-S` for sorting)

`-P` track a process when interacting with a path

Specific traces
---

`-e trace=ipc` track communication between processes (IPC)

`-e trace=memory` track memory syscalls

`-e trace=network` track memory syscalls

`-e trace=process` track process calls (like fork, exec)

`-e trace=signal` track process signal handling (like HUP, exit)

`-e trace=file` track file related syscalls

##### Monitoring file activity

    strace -e trace=file -p 1234

or

    strace -e trace=desc -p 1234

If you want to track specific paths, use 1 or more times the -P parameter, following by the path.

    sudo strace -P /etc/cups -p 2261

##### Monitoring the network

    trace -e trace=network

##### Monitoring memory calls

    strace -e trace=memory

Handy one-liners
---

##### Slow the target command and print details for each syscall:

    strace command

##### Slow the target PID and print details for each syscall:

    strace -p PID

##### Slow the target PID and any newly created child process, printing syscall details:

    strace -fp PID

##### Slow the target PID and record syscalls, printing a summary:

    strace -cp PID

##### Slow the target PID and trace open() syscalls only:

    strace -eopen -p PID

##### Slow the target PID and trace open() and stat() syscalls only:

    strace -eopen,stat -p PID

##### Slow the target PID and trace connect() and accept() syscalls only:

    strace -econnect,accept -p PID

##### Slow the target command and see what other programs it launches (slow them too!):

    strace -qfeexecve command

##### Slow the target PID and print time-since-epoch with (distorted) microsecond resolution:

    strace -ttt -p PID

##### Slow the target PID and print syscall durations with (distorted) microsecond resolution:

    strace -T -p PID

Common tasks
---

##### Find out which config files a program reads on startup

    strace php 2>&1 | grep php.ini
    strace -e open php 2>&1 | grep php.ini

##### Why does this program not open my file?

    strace -e open,access 2>&1 | grep your-filename

Look for an open() or access() syscall that fails

##### What is that process doing RIGHT NOW?

    strace -p <pid>

##### What is taking time?

    strace -c -p 11084

Sample output could be as follows:

    Process 11084 attached - interrupt to quit
    Process 11084 detached
    % time     seconds  usecs/call     calls    errors syscall
    ------ ----------- ----------- --------- --------- ----------------
    94.59    0.001014          48        21           select
    2.89    0.000031           1        21           getppid
    2.52    0.000027           1        21           time
    ------ ----------- ----------- --------- --------- ----------------
    100.00    0.001072                    63           total

exit with Ctrl+C

##### Why the **** can't I connect to that server?

    strace -e poll,select,connect,recvfrom,sendto nc www.drom.ru 80

Syscalls
---

| syscall | what it does                                         |
|---------|------------------------------------------------------|
| read    | read bytes from a file descriptor (file, socket)     |
| write   | write bytes from a file descriptor (file, socket)    |
| open    | open a file (returns a file descriptor)              |
| close   | close a file descriptor                              |
| fork    | create a new process (current process is forked)     |
| exec    | execute a new program                                |
| connect | connect to a network host                            |
| accept  | accept a network connection                          |
| stat    | read file statistics                                 |
| ioctl   | set I/O properties, or other miscellaneous functions |
| mmap    | map a file to the process memory address space       |
| brk     | extend the heap pointer                              |

Each call has a `man` page
