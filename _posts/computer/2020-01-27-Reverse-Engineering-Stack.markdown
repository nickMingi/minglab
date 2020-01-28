---
layout: "post"
title: "Reverse Engineering Stack"
author: "mingi hong"
date: 2020-01-27 17:00:24 -0700
categories: "Computer ReverseEngineering"
permalink: /:categories/:title.html
---

## Stack

1. You push things on
    - Registers
    - Immediates
    - References
2. You pop things off
    - Registers
    - Memory addresses
3. Always grows towards lower memory addresses
    - EBP = 0x00000124
        - sub esp, 0x24
    - ESP = 0x00000100
4. Access memory on the stack
    - ESP+<offset>
    - EBP-<offset>

So, Let's say prologue

{% highlight asm %}
push ebp         // saving old address
mov ebp, esp     // moving esp
sub esp, 0x20    // 0x20 is hex 20(EBP)
{% endhighlight%}

5. Stack grows up or grows down
    - Always depend what your memory layout is
    - If larger memory is up -> grows down
    - If larger memory is down -> grows up
    - But doesn't change functionality
6. Avoid this at all costs
    - 
7. Stack Frames
    - Return addresses
    - Arguments to functions
    - Local variables
    - Save register states

# Real C Program into stack
- If you call the function with arguments(Stack frame)
    - Must pass arguments to function
    - Must preserve order
    - Must preserve calling function state
    - Must return from function call
- Calling Conventions
    - Who
        - Pushes onto the stack
        - Pops off the stack
    - What
        - Registers are 'safe' or 'unsafe'
        - Where

# X86 and X64 Differences
1. X86 calling convention variations
    - Registers preserved
    - Volatile registers
    - Callee vs caller stack cleanup
2. Gets confusing and frustrating
3. Red zone
4. Shadow space
