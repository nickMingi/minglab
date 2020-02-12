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

- CS - mutual exclusion 
{% highlight c %}
    - do{ entry section
            // critical section
          exit section
            // remainder section
    }while(1);
{% end highlight %}

1. mutual exclusion: only 1 process in the CS at a time
2. progress: if the cs is empty, and there is a process that want into the cs, only processes not in their remainder section can have a say on who gets into the cs. AND the selection must not be postponed indefinitely. 
3. bounded wait: there exists a bound on the number of times a process asking to be let into the cs is not picked.

- Two process solution

# Multi - process solution to CS problem

Bakery

Algorithm

