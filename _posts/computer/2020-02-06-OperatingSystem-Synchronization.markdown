---
layout: "post"
title: "Process Synchronization"
author: "mingi hong"
date: 2020-02-06 12:24:29 -0700
categories: "Computer OperatingSystem"
permalink: /:categories/:title.html
---

## Process Synchronization

1. Cooperating Processes
    - Directly share space
    - Share a file <-> DB
2. Producer

# Race Condition
- A situation like this, where several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the access takes place, is called a race condition.

- we want any changes that result from such activities not to interfere with one another. Because of the importance of this issue, we devote a major portion of this chapter to process synchronization and coordination among cooperating processes.

- Critical section model

# Critical Section Problem

- a section of code where the processes may be changing common data Critical Section

![OSscreen](/minglab/assets/OSscreen.png)

# Difference between Concurrency and Parallelism
- Structue and implementation

# Syncronization in Hardware
{% highlight c++ %}
 Test & Set: boolean test&set (boolean &target)
    {
        boolean rv = target;
        target > true;
        return rv;
    }
    boolean lock = false;
    do{
        while(Test&set(lock));
            cs
        lock = false;
            rs
    }while(1);
{% end highlight %}

This is just showing hardware logic.

circuit tree 

{% highlight c++ %}
compare-and-swap
int compare-swap(int &value, int expected, int new_value)
{
    int temp = value;
    if(value == expected)
        value = new_value;
    return temp;
}

int lock = 0;
do{
    while(compare-swap(lock,0,1)!=0);
        cs
    lock = 0;
        rs
}while(true);
{% end highlight %}

This is hardware logic

![hardwaresync](/minglab/assets/HardwareSync.png)

# Mutex Lock
- aquire()
- release()
{% highlight c++ %}
aquire()
{
    while(!available); // here we get busy wait(spin lock)
    available = false;
}
release()
{
    available = true;
}
{% end highlight %}

{% highlight c++ %}
do{
    aquire(lock);
        cs
    release(lock);
        rs
}while(1);
{% end highlight %}

# Semaphore
- Simplest of synchronization tool
- int S
- wait(s) : {while(s<=0);//busy wait s--;}
- signal(s) : {s++;}
- wait and signal operations must be executed indivisibly.
- There are counting and binary semaphores.
    - counting semaphore : can range over an unrestricted domain
    - binary semaphore : can range only between 0 and 1

# Non-busy wait semaphore
{% highlight c %}
    // we can handle busy wait
    type def
    {
        int value;
        struct process *L;
    }
    wait(semaphore &s)
    {
        s-> value--;
        if(s->value<0)
        {
            add process to s->L;
            block();
        }   
    }
    signal(semaphore &s)
    {
        s->value++;
        if(s->value<=0)
        {
            remove process from s->L;
            wakeup(p);
        }
    }
{% end highlight %}

