---
layout: "post"
author: "mingi"
title: "CPU and Midterm"
categories: "Computer OS"
permalink: /:categories/:title.html
---

## CPU

When you have a process, have to use CPU, you have to wait

Move process out from CPU just sitting, It can work while waiting.

CPU sit IDLE, DO IO, you want to solve IDLE clock cycle

That's why we use concurrency

# EXAM
- textbook not responsible for
    - 3.5(all)
    - 4.4(all),4.5(all), 4.5.6, 4.7(all)
    - All of 5.7,5.8,5.9,5.10

1. Forks
    - where child start after the fork? (Same place)
        - Right after fork
        - duplicate of child and starts at same place
        - make copy of PCB (Why make it starts at same place)
    - 

2. Critical section
    - mutual exclusion
    - processes
    - bounded wait

3. Concurrency
    - why do we use it?
        - Efficiency 
    - what is the difference between concurrency and parallism

4. Process concept
    - A process is the unit of work in a modern time-sharing system.
    - A process is more than the program code.
    - A program becomes a process when an executable file is loaded into memory.
    - Java Virtual Machine executes as a process that interprets the loaded Java code and 
    takes actions on behalf of that code.
    - Process States can be described as five
        - New: the process is being created.
        - Running: Instructions are being executed.
        - Waiting: the process is waiting for some event to occur 
        (such as an I/O completion or reception of a signal)
        - Ready: the process is waiting to be assigned to a processor.
        - Terminated: the process has finished execution
    - Each process is represented in the OS by a PCB(process control block) or 
    TCB(task control block)
    - Process state: the state may be new,ready,running,waiting,halted,and so on.
    - Program counter: the counter indicates the address of the next instruction to be executed for this process
    - CPU registers: the registers vary in number and type, depending on the computer architecture.
    - CPU-scheduling information: this information includes a process priority, pointers to scheduling queues, and any other scheduling parameters.
    - Memory-management information: this information may include such items as the value of the base and limit registers and the page tables, or the segment tables, depending on the memory system used by the operating system
    - Accounting information: 
    - I/O status information:

5. Process scheduling
    - The objective of multiprogramming is to have some process running at all times, to maximize CPU utilization. 
    - To meet these objectives, the process scheduler selects an available process for program execution on the CPU. 
    - As processes enter the system, they are put into a job queue
    
6. Operations on processes
    - The processes in most systems can execute concurrently, and they may be created and deleted dynamically. Thus, these systems must provide a mechanism for process creation and termination. 
    - The creating process is called a parent process
    - New processes are called the children of that process
    - Each process has unique process identifier(pid)
    - When a process creates a child process, that child process will need certain resources(CPU time, memory, files, I/O devices) to accomplish its task.
    - When a process creates a new process, two possibilities for execution exist:
        - The parent continues to execute concurrently with its children
        - The parent waits until some or all of its children have terminated
    - There are also two address-space possibilities for the new process
        - The child process is a duplicate of the parent process
        - The child process has a new program loaded into it
    - A new process is created by the fork()
    - The new process consists of a copy of the address space of the original process.
    - The exec() system call loads a binary file into memory and starts its execution.
    - A parent may terminate the execution of one of its children for a variety of reasons, such as these:
        - The child has exceeded its usage of some of the resources that it has been allocated.(To determine whether this has occurred, the parent must have a mechanism to inspect the state of its children)
        - The task assigned to the child is no longer required
        - The parent is exiting, and the operating system does not allow a child to continue if its parent terminates
    - Cascading termination (If parent died, all of children dye)

7. Interprocess communication
    - Allowing process cooperation
        - Information sharing
        - Computation speedup
        - Modularity
        - Convenience 
    - Shared memory: a region of memory that is shared by cooperating processes is established.
    - Messeage passing: communication takes place by means of messages exchanged between the cooperating processes. 
    - Shared memory can be faster than message passing, since message-passing systems are typically implemented using system calls and thus require the more time-consuming task of kernel intervention.
![ipcexample](/minglab/assets/ipcexample.png)
   
   - As the number of processing cores on systems increases, it is possible that we will see message passing as the preferred mechanism for IPC
   - Only fixed sized messages system-level implementation is straightforward. But makes the task of programming more difficult
   - Only variable sized messages require a more complex system level implementation but the programming task becomes simpler.
   - logically implementing a link and the send/ receive
    -   Direct or indirect 
    - synchronous or asynchronous
    - automatic or explicit buffering
   - Blocking send: the sending process is blocked until the message is received by the receiving process or by the mailbox
   - Nonblocking send: the sending process sends the message and resumes operation
   - Blocking receive: the receiver blocks until a message is available
   - Nonblocking receive: the receiver retrieves either a valid message or a null

8. Communication in client-server systems
    - Sockets
        - Defined as an endpoint for communication
        - Connection-oriented(TCP)
        - Connectionless(UDP)
    - Remote procedure calls(RPCs)
        - Another form of distributed communication
        - Binding information may be predetermined
        - Binding can be done dynamically by a rendezvous mechanism
        - RPC scheme is useful in implementing a distributed file system. 
    - Pipes
        - Provide a relatively simple ways for processes to communicate with one another.
        - A pipe acts as a conduit allowing two processes to communicate.
        - Four issues
            - pipe allow bidirectional? or unindirectional?
            - two way is allowed, half duplex? full duplex?
            - Must relationship exist?
            - over a network? or reside on the same machine?
        - Ordinary pipes
            - This is unidirectional, only one way
            - This exist only while the processes are communicating with one another.
        - Named pipes
            - It provide a much more powerful communication tool. Communication can be bidirectional, and no parent-child relationship is required.
            - It is referred to as FIFOs in UNIX
            - Even though FIFO allows bidirectional, it is half duplex.
            - The communicating processes must reside on the same machine.
            - If intermachine communication is required, sockets must be used.
            - It is a richer communication mechanism on Windows than Unix. 
            - Full duplex is allowed 
            - Communicating processes reside  on either the same or different machines. 
            - Either byte or message oriented data allowed 

9. Multicore programming

10. Multithreading models

11. The critical-section problem

12. Peterson's solution

13. Synchronization hardware

14. Mutex locks

15. Semaphores

16. Spin lock -> busy waiting

17. Deadlock

# Homework
- Explain why spinlocks are not appropriate for single-processor systems yet
often used in multiprocessor systems
    - Spinlocks are not appropriate for single-processor systems
    because the condition that would break a process out of the spinlock
    could be obtained only by executing a different process. If
    the process is not relinquishing the processor, other processes
    do not get the opportunity to set the program condition
    required for the first process to make progress. In a
    multiprocessor system, other processes execute on other
    processors and thereby modify the program state in order
    to release the first process from the spinlock.
- Explain why implementing synchronization primitives by disabling
interrupts is not appropriate in a single processor system if
the synchronization primitives are to be used in user-level programs
    - If a user-level program is given the ability to disable
    interrupts, then it can disable the timer interrupt and prevent
    context switching from taking place, thereby allowing it to use
    the processor without letting other processes to execute.
![trafficdeadlock](/minglab/assets/trafficdeadlock.png)
- The four necessary conditions for a deadlock are
    1. mutual exclusion
        - holds as only one car can occupy a space in the roadway
    2. hold and wait
        - occurs where a car holds onto their
        place in the roadway while they
        wait to advance in the roadway
    3. no preemption
        - A car cannot be removed from its position in the roadway
    4. circular wait
        - circular wait as each car is waiting for a subsequent car to advance
        The circular wait condition is also easily observed from the graphic.
- A simple rule that would avoid this traffic deadlock
is that a carmay not advance into an intersection if
it is clear they will not be able to immediately
clear the intersection
    - A simple rule that would avoid this traffic deadlock
    is that a carmay not advance into an intersection
    if it is clear they will not be able to
    immediately clear the intersection
- Peterson's algorithm

```
do{
    flag[i] = true;
    turn = j;
    while(flag[j] && turn == j);
        cs
    flag[i] = false;
        rs
}while(1);

```

Item     | Flag | Turn
---------|------|------
mutex    | N    | Y
progress1| Y    | N
progress2| Y    | N
bounded  | N    | Y

- Banker's Algorithm
    1. Available has to be equal or bigger than request
    2. Calculate Need which is Max - Allocated
    3. Need has to be equal or bigger than request

