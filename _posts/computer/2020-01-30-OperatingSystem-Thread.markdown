---
layout: "post"
title: "Operating System Thread"
author: "mingi hong"
date: 2020-01-30 13:12:33 -0700
categories: "Computer Operatingsystem"
permalink: /:categories/:title.html
---

## Thread

# A fundamental unit of CPU utilization

Those are have to be added to PCB

1. It has to have own thread ID
2. Keep track program counter
3. Have to own register set
4. Stack(Have to know where i was)

What has to be shared?
- code section
- data section
- other OS resources
    - file handles
    - signal..

![ThreadProcess](/minglab/assets/ThreadProcess.png)

Benefits
    - Responseiveness 
    - Resource sharing
    - Economy
    - Scalability

# Multicore Programming
- multicore(On same ship) / multi processor 
- each appears as separate processor to OS

Parallelism / Concurrency
- Parallel system : More then 1 task at same time
- Concurrent system : All task's make "progress"

- Hardware support increasing for multiple cores 2, 4, 8

Programming Challenges(multicore)
- Ident: fying Tasks
    - How do we break up a process
- Balance
    - How to distribute tasks across cores
- Data splitting
    - How to place data[where physically]
- Data dependency
    - Managing data shared by tasks
- Testing and debugging
    - Concurrency add big increase in complexity

# Types of Parallelism
- Data 
- Task

1. Data Parallelism
    - distribution of subsets of same data across multiple cores and then performing the same operation on each.
    - same function

2. Task Parallelism
    - different function
    - distribution of tasks(threads) across multiple cores. Each task is performing a unique operation
    - same data

- Most common to have a hybrid approach as location of data can have by impact on task performance

# Example
- 6 cores mean 6 processors
- 6 threads mean could be one hardware threads or six hardware threads.

# MultiThread Models
- User Threads(User application simulating multithread, No support from kernel)
- Kernel Threads(Management and support from the kernel)

1. One-To-One Model - kernel threads
2. Many-To-One Model - user threads (many user -> 1 kernel)
3. Many-To-Many 

# Thread Libraries
- User an API for management Thread
