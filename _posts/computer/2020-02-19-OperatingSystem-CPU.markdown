---
layout: "post"
author: "mingi"
title: "CPU"
categories: "Computer OS"
permalink: /:categories/:title.html
---

## CPU

When you have a process, have to use CPU, you have to wait

Move process out from CPU just sitting, It can work while waiting.

CPU sit IDLE, DO IO, you want to solve IDLE clock cycle

That's why we use concurrency

# EXAM
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
    