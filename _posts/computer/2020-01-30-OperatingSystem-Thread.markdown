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