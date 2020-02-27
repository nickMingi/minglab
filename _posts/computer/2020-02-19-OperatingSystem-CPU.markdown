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
    - A thread is a basic unit of CPU utilization; 
    - If the web server ran as a traditional single-threaded process, it would be able to service only one client at a time, and a client might have to wait a very long time for its request to be serviced.
    - Four Benefit of multithreaded programming
        - Responsiveness
        - Resource sharing
        - Economy
        - Scalability
    - A single-threaded process can run on only one processor, regardless how many are available.

10. Multithreading models
    - Processing core is capable of executing only one thread at a time
    - On a system with multiple cores, concurrency means that the threads can run in parallel, because the system can assign a separate thread to each core.
    - A system is parallel if it can perform more than one task simultaneously.
    - A concurrent system supports more than one task by allowing all the tasks to make progress.
    - It is possible to have concurrency without parallelism
    - Five areas present challenges
        - Identifying tasks
        - Balance
        - Data splitting
        - Data dependency
        - Testing and debugging
    - There are two types of parallelism
        - Data parallelism
        - Task parallelism
    - Kernel threads : supported and managed directly by the OS
    - User threads : supported above the kernel and are managed without kernel support
    - Many-to-One model : One kernel many user threads, It has inability to take advantage of multiple processing cores.
    - One-to-One model : It provides more concurrency than the many-to-one, overhead of creating kernel threads can burden the performance of an application.
    - Many-to-many model : many user-level threads to a smaller or equal number of kernel threads. 
    - Kernel can schedule only one thread at a time.
    - Even though one-to-one model allows greater concurrency, developer has to be careful not to create too many threads within an application.
    - Many-to-Many model suffers from neither these shortcomings.
    - Many user-level threads to a smaller or equal number of kernel threads bul also allows a user-level thread to be bound to a kernel thread. called two-level model.
    - User level threads are faster to create and manage than are kernel threads, because no intervention from the kernel is required.
11. The critical-section 
    - Several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the access takes place, is called a race condition.
    - To guard against the race condition, we need to ensure that only one process at a time can be manipulating the variable counter. which is process synchronization
    - Critical-section: no two processes are executing in their critical sections at the same time.
    - Each process must request permission to enter its critical section.
    - It consist of entry section, remainder section, and exit section.
    - Critical section problem must satisfy three requirements
        - Mutual exclusion: If one process is executing in its critical section, then no other processes can be executing in their critical sections.
        - Progress: If no process is executing in its critical section and some processes wish to enter their critical sections, then only those processes that are not executing in their remainder sections can participate in deciding which will enter its critical section next, and this selection cannot be postponed indefinitely.
        - Bounded waiting: There exists a bound, or limit, on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.
        - there are two general approaches 
            - preemptive kernels
                - allows a process to be preempted 
            - nonpreemptive kernels
                - does not allow a process running to be preempted
        - Obviously, a nonpreemptive kernel is essentially free from race conditions
        - But preemptive kernel is popular since it is more responsive, suitable for real-time programming
12. Peterson's solution
    - The variable turn indicates whose turn it is
    to enter its critical section.
    - The flag array is used to indicate if a process
    is ready to enter its critical section.
    - Turn determines which of the two processes is allowed
    to enter its critical section first
    - Pi does not change the value of the variable
    turn while executing the while statement 
    Pi will enter the critical section(progress)
    after at most one entry by Pj(bounded waiting)

13. Synchronization hardware
    - atomically : as one uninterruptible unit
    - we note that process Pi can enter its critical section
    only if either waiting[i]==false or key==false
    - The value of key can become false only if
    the test_and_set() is executed.

14. Mutex locks
    - simplest of synchronization tools.
    - We use mutex lock to protect critical regions
    and thus prevent race conditions.
    - A process must acquire the lock before 
    entering a critical section; It releases
    the lock when it exits the critical section.
    - acquire() or release() must be performed atomically
    - it requires busy waiting also called spinlock
    - Busy waiting wastes CPU cycles that some other
    process might be able to use productively.
    - spinlock do have an advantage no context switch
    is required when a process must wait on a lock
    and a context switch may take considerable time.

15. Semaphores
    - more robust tool than mutex lock also provide
    more sophisticated ways.
    - all modifications to the integer value of the 
    semaphore in the wait() and signal() operations
    must be executed indivisibly.
    - when one process modifies the semaphore value,
    no other process can simultaneously modify that
    same semaphore value.
    - There are two semaphores
        - counting semaphore
        - binary semaphore
    - Binary semaphore behave similarly to mutex locks
    - Semaphores has same problem as mutex lock
    - To overcome the need for busy waiting, we can
    modify the definition of the wait() and signal()
    - Use block operation, control is transferred to the
    CPU scheduler, which selects another process to execute.
    - wakeup operation resumes the execution of a 
    blocked process.
    - But, it can be negative while busy waiting is not
    - It is critical that semaphore operations be
    executed atomically. We must guarantee that
    no two processes can execute wait() and signal()
    
16. Spin lock -> busy waiting

17. Deadlock
    - The implementation of a semaphore with a
    waiting queue may result in a situation where 
    two or more processes are waiting indefinitely
    for an event that can be caused only by one
    of the waiting processes.
    - Indefinite blocking or starvation problem
    - A situation in which processes wait indefinitely 
    within the semaphore. Indefinite blocking may
    occur if we remove processes from the list
    associated with a semaphore in LIFO order
    - Priority inversion occurs only in systems
    with more than two priorities, so one solution
    is to have only two priorities.
    - But typically these systems solve the problem
    by implementing a priority-inheritance protocol.
    - A process requests resources if the resources
    are not available at that time, the process
    enters a waiting state. Sometimes, a waiting process is
    never again able to change state, because the
    resources it has requested are held by other
    waiting processes. 
    - A process may utilize a resource in only 
        - Request: if request cannot be granted 
        immediately, then the requesting process
        must wait until it can acquire the resource
        - Use: Process can operate on the resource
        - Release: Process releases the resource
    - Deadlock can arise with four conditions
        - Mutual exclusion: Only one process at a time
        can use the resource
        - Hold and wait: Process must be holding at
        least one resource and waiting to acquire
        additional resources that are currently
        being held by other processes
        - No preemption: Resource can be released
        only voluntarily by the process holding it
        after that process has completed its task
        - Circular wait: remember the intersection
    - Handling deadlocks
        - use a protocol to prevent or avoid deadlcks
        ensuring that the system will never enter
        a deadlocked state
        - allow the system to enter a deadlocked state,
        detect it, and recover.
        - ignore the problem altogether and pretend
        that deadlocks never occur in the system
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

