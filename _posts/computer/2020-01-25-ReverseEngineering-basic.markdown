---
layout: "post"
title: "ReverseEngineering basic"
author: "mingi hong"
date: 2020-01-25 15:24:59 -0700
categories: Computer ReverseEngineering
permalink: /:categories/:title.html
---

## Assembly Language

Look for ELF

control + z = stop processing with terminal

{% highlight asm %}
0xBA = EDX
0xB9 = ECX
0xBB = EBX
0xB8 = EAX

mov eax
move ebx
add tax,ebb     addition
sub tax,0x2C    subtract
amp eat,150     compare then set zero flag
jle _lesser      //jump if equal zero flag
jmp _continue:

_lesser:

_continue:
-F
_exit:
{% endhighlight %}

# Terminology
1. nasm -f elf32 firstAsmStripped.asm
2. nasm -f elf32 -g -F dwarf firstAsmStripped.asm
3. ld -m elf_i386 firstAsmStripped.o -o firstAsmStripped
4. kdbg firstAsmStripped &

Debugging mode

5. readily -a firstAsmStripped
6. MAC -> nasm -f macho64 64.asm
7. MAC -> ld -macosx_version_min 10.7.0 -lSystem -o 64 64.o

## Reverse Engineering

# Terminology
1. nasm -f elf test.asm
    - f is indicating format
2. ld -m elf_i386 -o test test.o
    - m is how to link to create a binary file ( 32 bit, 64 bit, ...)
    - o is output file
3. xxd test.asm
    - To see hexadecimal information of object file
    - Also can be used for xxd test.o

The beginning of file looks like

![xxdoutput](/minglab/assets/xxdoutput.png)

- What type of this file? elf? OS has to know this.
- What is this program?
- It is interpreted into hex values
- Step away from binary(we need one more step)

4. objdump - M intel -d test.o(UBUNTU) == objdump -source test.o(MAC)

![objdumpoutput](/minglab/assets/objdumpoutput.png)

5. ld is the last command to make it binary file but can not be run on mac(32bit)

# The difference between compiler made code and handmade .asm

1. assembly file
    - we know how it is going
2. compiler wrote file(gcc maybe)
    - machine are creating code that not friendly to humans
    - they create prologue & epilogue
        - they adjust the stack -> you can successfully return to the original value
    - What's the stack?

# Reviewing terminology 

1. What security protection does DEP/NX offer?
    - Data Execution Prevention / Non Execution.
    - 
2. What security protection does ASLR offer?
    - Adress Space Layout Randomization
    - Randomly loads program in the memory
    - 
3. What are stack canaries and what security protection do they offer
    - secret values put on stack
    - 0xDEADBEEF (Stack canaries)
    - If it does not match with stack when you start program
    - it makes crash.
4. What is SEH and what does it do?
    - Structure exception handling
    - Mostly windows thing. Structure contains exception 
    - Will go look up SEH if there is no user entry, exception handler execute that.
    - 
5. What is SUID and what does it do?
    - Set User I D
    - Mostly Unix/Linux term
    - Let you attach permission to binary
    - But not actually give user root permision
    - Kind of bad idea.
6. What does BOF stand for? What are 2 common types of BOFs? 
    - Buffer overflow
    - Stack and heap
7. What is ROP?
    - Return Oriented Programming
    - Searching for gadget somehow manipulate memory or register and returns
    - Method of programming
8. What are ROP Gadgets?
    - Assembly instructions
9. Briefly describe what shellcode is
    - Part of the program that does the work
    - Functionality what your exploit does
10. What is an egg in computer security terms? Why would you use an egg?
    - Unique Identifier to mark memory somehow
    - You will find BOF, but it only contains 50 bytes of memory. What would i do is replace it to egg.
11. What is an egg hunter?
    - Very small chunk of code to find egg in memory.
