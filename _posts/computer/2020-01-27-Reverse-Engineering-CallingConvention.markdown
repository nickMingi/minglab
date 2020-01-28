---
layout: "post"
title: "Reverse Engineering Calling Convention"
author: "mingi hong"
date: 2020-01-27 15:00:24 -0700
categories: "Computer ReverseEngineering"
permalink: /:categories/:title.html
---

## Calling Conventions

1. How are arguments passed between functions?
2. Who is responsible for what?
    - Caller responsible for cleanup, or callee?
    - Does system clean everything up?
3. Different OSs/architectures do things differently
4. Times change methods

# Caller clean-up
- Cdecl
- Primary method for linux compilers to compile C declaration
- Arguments passed on the stack
- Return values in EAX
- Caller saves: EAX, ECX, EDX. Callee saves everything else
- Function arguments pushed right to left
- Usually default for X86 C compilers
    - But not guaranteed to be portable

# Callee clean-up
- stdcall
- Microsoft fastcall
- Parameters are pushed right-to-left
- EAX, ECX, EDX are not safe(can be used by the function)
- Return values stored in EAX
- Microsoft Win32API
- CALLEE CLEANS UP THE STACK
    - 'ret 0x8'
    - Two words off the stack
- Passes first two arguments(left to right) in ECX, EDX.
- Additional arguments pushed onto stack right to left.

# Others
- thiscall
- 

# 64 bit Windows
- RCX,RDX,R8,R9 for first 4 arguments
    - Other arguments pushed onto stack right to left
- Return in RAX
- 32 bytes 'Shadow Space' allocated by caller on stack before calling function and pop stack after call.
- Caller-saved: RAX,RCX,RDX,R8-R11 (volatile)
- Callee-saved: RBX,RBP,RDI,RSI,RSP,R12-R15(nonvolatile)

# 64 bit Linux(System V AMD64 ABI)
- RDI,RSI,RDX,RCX,R8,R9 for first 6 arguments.
    - Other arguments pushed onto stack right to left
- Return in RAX
- r10 used for system calls
- Caller-saved: RAX,RCX,RDX,R8-R11(volatile)
- Callee-saved: RBX,RBP,RSP,R12-R15(nonvolatile)

# thiscall
- C++ special
- GCC
    - Pushes 'this' pointer onto the stack last; as if it was a 'hidden' first argument
    - Very similar to cdecl
- Microsoft
    - 'this' pointer passed in ECX, and the callee cleans the stack. (similar to stdcall)
    - If variable number of arguments, the caller cleans the stack (similar to cdecl, again.)

# ARM 32bit
- 15 general purpose registers
- r15 PC, r14 LR, r13 SP, r12 Intra-procedure-call scratch register
- r4-r11 holds local variables; r0-r3 hold arguments and return value
- Callees must preserve r4-r11 and SP.
    - Callee MUST save LR to stack before calling other function

