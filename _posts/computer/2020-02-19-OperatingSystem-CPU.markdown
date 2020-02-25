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

2. Critical section
    - mutual exclusion
    - processes
    - bounded wait

3. Concurrency
    - why do we use it?
    - what is the difference between concurrency and parallism

4. Process concept

5. Process scheduling

6. Operations on processes

7. Interprocess communication

8. Communication in client-server systems

9. Multicore programming

10. Multithreading models

11. The critical-section problem

12. Peterson's solution

13. Synchronization hardware

14. Mutex locks

15. Semaphores

16. Spin lock -> busy waiting

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

