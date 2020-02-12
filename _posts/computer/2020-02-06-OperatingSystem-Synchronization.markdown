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



