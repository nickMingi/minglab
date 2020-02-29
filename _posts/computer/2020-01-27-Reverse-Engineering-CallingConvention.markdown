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

{% highlight asm %}
section .text
    global _start

_start:
    call _main
    mov ebx, eax ; exit system call
    mov eax, 0x1 ; exit system call
    int 0x80     ; execute system call

_main:
    push ebp
    mov ebp, esp
    sub esp, 0x10 ; moving esp value further down the stack
; Here it is prologue of our program
; now we can do our program 'stuff'
; And we need to create epilogue
    add esp, 0x10 ; give back that stack to the system
    pop ebp 
    ret

; peda
; Python Exploit Development Assistance for GDB
; disas _main
; br *0x0804806e
; br _main
; del br 2
; info br
; r
; display/20xw $esp
; si 

{% endhighlight %}

# Advanced

- objdump -disassembly-all 
- will show you assembly code but not quite .asm
- We can see custom function as well

{% highlight asm %}
populateArray: b2,3
 8048426:	55 	pushl	%ebp
 8048427:	89 e5 	movl	%esp, %ebp
 8048429:	83 ec 10 	subl	$16, %esp
 804842c:	e8 b8 00 00 00 	calll	184 <__x86.get_pc_thunk.ax>
 8048431:	05 cf 1b 00 00 	addl	$7119, %eax
 8048436:	c7 45 fc 00 00 00 00 	movl	$0, -4(%ebp)
 804843d:	c6 45 fb 61 	movb	$97, -5(%ebp)
 8048441:	c7 45 fc 00 00 00 00 	movl	$0, -4(%ebp)
 8048448:	eb 20 	jmp	32 <populateArray+0x44>
 804844a:	8b 55 fc 	movl	-4(%ebp), %edx
 804844d:	8b 45 08 	movl	8(%ebp), %eax
 8048450:	01 c2 	addl	%eax, %edx
 8048452:	0f b6 45 fb 	movzbl	-5(%ebp), %eax
 8048456:	88 02 	movb	%al, (%edx)
 8048458:	8b 45 fc 	movl	-4(%ebp), %eax
 804845b:	89 c2 	movl	%eax, %edx
 804845d:	0f b6 45 fb 	movzbl	-5(%ebp), %eax
 8048461:	01 d0 	addl	%edx, %eax
 8048463:	88 45 fb 	movb	%al, -5(%ebp)
 8048466:	83 45 fc 01 	addl	$1, -4(%ebp)
 804846a:	8b 45 fc 	movl	-4(%ebp), %eax
 804846d:	3b 45 0c 	cmpl	12(%ebp), %eax
 8048470:	7c d8 	jl	-40 <populateArray+0x24>
 8048472:	b8 00 00 00 00 	movl	$0, %eax
 8048477:	c9 	leave
 8048478:	c3 	retl
{% endhighlight %}

{% highlight asm %}
getRandomLetter: b3
 8048509:	55 	pushl	%ebp
 804850a:	89 e5 	movl	%esp, %ebp
 804850c:	53 	pushl	%ebx
 804850d:	83 ec 14 	subl	$20, %esp
 8048510:	e8 b2 00 00 00 	calll	178 <__x86.get_pc_thunk.ax>
 8048515:	05 eb 1a 00 00 	addl	$6891, %eax
 804851a:	89 c3 	movl	%eax, %ebx
 804851c:	e8 5f fe ff ff 	calll	-417 <rand@plt>
 8048521:	99 	cltd
 8048522:	f7 7d 0c 	idivl	12(%ebp)
 8048525:	89 55 f4 	movl	%edx, -12(%ebp)
 8048528:	8b 55 f4 	movl	-12(%ebp), %edx
 804852b:	8b 45 08 	movl	8(%ebp), %eax
 804852e:	01 d0 	addl	%edx, %eax
 8048530:	0f b6 00 	movzbl	(%eax), %eax
 8048533:	83 c4 14 	addl	$20, %esp
 8048536:	5b 	popl	%ebx
 8048537:	5d 	popl	%ebp
 8048538:	c3 	retl
{% endhighlight %}

{% highlight asm %}
populateArray: b4
 8048516:	55 	pushl	%ebp
 8048517:	89 e5 	movl	%esp, %ebp
 8048519:	56 	pushl	%esi
 804851a:	53 	pushl	%ebx
 804851b:	83 ec 10 	subl	$16, %esp
 804851e:	e8 2d ff ff ff 	calll	-211 <__x86.get_pc_thunk.bx>
 8048523:	81 c3 dd 1a 00 00 	addl	$6877, %ebx
 8048529:	e8 b2 fe ff ff 	calll	-334 <rand@plt>
 804852e:	89 c1 	movl	%eax, %ecx
 8048530:	ba 1f 85 eb 51 	movl	$1374389535, %edx
 8048535:	89 c8 	movl	%ecx, %eax
 8048537:	f7 ea 	imull	%edx
 8048539:	c1 fa 05 	sarl	$5, %edx
 804853c:	89 c8 	movl	%ecx, %eax
 804853e:	c1 f8 1f 	sarl	$31, %eax
 8048541:	29 c2 	subl	%eax, %edx
 8048543:	89 d0 	movl	%edx, %eax
 8048545:	89 45 f0 	movl	%eax, -16(%ebp)
 8048548:	8b 45 f0 	movl	-16(%ebp), %eax
 804854b:	6b c0 64 	imull	$100, %eax, %eax
 804854e:	29 c1 	subl	%eax, %ecx
 8048550:	89 c8 	movl	%ecx, %eax
 8048552:	89 45 f0 	movl	%eax, -16(%ebp)
 8048555:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 804855c:	c7 45 ec 00 00 00 00 	movl	$0, -20(%ebp)
 8048563:	8b 45 f0 	movl	-16(%ebp), %eax
 8048566:	c1 e0 02 	shll	$2, %eax
 8048569:	83 ec 0c 	subl	$12, %esp
 804856c:	50 	pushl	%eax
 804856d:	e8 2e fe ff ff 	calll	-466 <malloc@plt>
 8048572:	83 c4 10 	addl	$16, %esp
 8048575:	89 c2 	movl	%eax, %edx
 8048577:	8b 45 08 	movl	8(%ebp), %eax
 804857a:	89 10 	movl	%edx, (%eax)
 804857c:	8b 45 f0 	movl	-16(%ebp), %eax
 804857f:	c1 e0 03 	shll	$3, %eax
 8048582:	83 ec 0c 	subl	$12, %esp
 8048585:	50 	pushl	%eax
 8048586:	e8 15 fe ff ff 	calll	-491 <malloc@plt>
 804858b:	83 c4 10 	addl	$16, %esp
 804858e:	89 45 e8 	movl	%eax, -24(%ebp)
 8048591:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 8048598:	eb 3c 	jmp	60 <populateArray+0xc0>
 804859a:	e8 41 fe ff ff 	calll	-447 <rand@plt>
 804859f:	89 c1 	movl	%eax, %ecx
 80485a1:	8b 45 f4 	movl	-12(%ebp), %eax
 80485a4:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 80485ab:	8b 45 e8 	movl	-24(%ebp), %eax
 80485ae:	8d 34 02 	leal	(%edx,%eax), %esi
 80485b1:	ba d3 4d 62 10 	movl	$274877907, %edx
 80485b6:	89 c8 	movl	%ecx, %eax
 80485b8:	f7 ea 	imull	%edx
 80485ba:	c1 fa 06 	sarl	$6, %edx
 80485bd:	89 c8 	movl	%ecx, %eax
 80485bf:	c1 f8 1f 	sarl	$31, %eax
 80485c2:	29 c2 	subl	%eax, %edx
 80485c4:	89 d0 	movl	%edx, %eax
 80485c6:	69 c0 e8 03 00 00 	imull	$1000, %eax, %eax
 80485cc:	29 c1 	subl	%eax, %ecx
 80485ce:	89 c8 	movl	%ecx, %eax
 80485d0:	89 06 	movl	%eax, (%esi)
 80485d2:	83 45 f4 01 	addl	$1, -12(%ebp)
 80485d6:	8b 45 f0 	movl	-16(%ebp), %eax
 80485d9:	01 c0 	addl	%eax, %eax
 80485db:	39 45 f4 	cmpl	%eax, -12(%ebp)
 80485de:	7c ba 	jl	-70 <populateArray+0x84>
 80485e0:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 80485e7:	eb 60 	jmp	96 <populateArray+0x133>
 80485e9:	e8 f2 fd ff ff 	calll	-526 <rand@plt>
 80485ee:	99 	cltd
 80485ef:	f7 7d f0 	idivl	-16(%ebp)
 80485f2:	89 d0 	movl	%edx, %eax
 80485f4:	01 c0 	addl	%eax, %eax
 80485f6:	89 45 ec 	movl	%eax, -20(%ebp)
 80485f9:	8b 45 ec 	movl	-20(%ebp), %eax
 80485fc:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 8048603:	8b 45 e8 	movl	-24(%ebp), %eax
 8048606:	01 d0 	addl	%edx, %eax
 8048608:	8b 00 	movl	(%eax), %eax
 804860a:	83 f8 ff 	cmpl	$-1, %eax
 804860d:	74 3a 	je	58 <populateArray+0x133>
 804860f:	8b 45 ec 	movl	-20(%ebp), %eax
 8048612:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 8048619:	8b 45 e8 	movl	-24(%ebp), %eax
 804861c:	8d 0c 02 	leal	(%edx,%eax), %ecx
 804861f:	8b 45 08 	movl	8(%ebp), %eax
 8048622:	8b 00 	movl	(%eax), %eax
 8048624:	8b 55 f4 	movl	-12(%ebp), %edx
 8048627:	c1 e2 02 	shll	$2, %edx
 804862a:	01 c2 	addl	%eax, %edx
 804862c:	8b 01 	movl	(%ecx), %eax
 804862e:	89 02 	movl	%eax, (%edx)
 8048630:	8b 45 ec 	movl	-20(%ebp), %eax
 8048633:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 804863a:	8b 45 e8 	movl	-24(%ebp), %eax
 804863d:	01 d0 	addl	%edx, %eax
 804863f:	c7 00 ff ff ff ff 	movl	$4294967295, (%eax)
 8048645:	83 45 f4 01 	addl	$1, -12(%ebp)
 8048649:	8b 45 f4 	movl	-12(%ebp), %eax
 804864c:	3b 45 f0 	cmpl	-16(%ebp), %eax
 804864f:	7c 98 	jl	-104 <populateArray+0xd3>
 8048651:	8b 45 f0 	movl	-16(%ebp), %eax
 8048654:	8d 65 f8 	leal	-8(%ebp), %esp
 8048657:	5b 	popl	%ebx
 8048658:	5e 	popl	%esi
 8048659:	5d 	popl	%ebp
 804865a:	c3 	retl
{% endhighlight %}

{% highlight asm %}
sortArray: b4
 804865b:	55 	pushl	%ebp
 804865c:	89 e5 	movl	%esp, %ebp
 804865e:	83 ec 10 	subl	$16, %esp
 8048661:	e8 c0 01 00 00 	calll	448 <__x86.get_pc_thunk.ax>
 8048666:	05 9a 19 00 00 	addl	$6554, %eax
 804866b:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 8048672:	c7 45 fc 00 00 00 00 	movl	$0, -4(%ebp)
 8048679:	e9 99 00 00 00 	jmp	153 <sortArray+0xbc>
 804867e:	c7 45 f8 00 00 00 00 	movl	$0, -8(%ebp)
 8048685:	eb 7d 	jmp	125 <sortArray+0xa9>
 8048687:	8b 45 f8 	movl	-8(%ebp), %eax
 804868a:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 8048691:	8b 45 08 	movl	8(%ebp), %eax
 8048694:	01 d0 	addl	%edx, %eax
 8048696:	8b 10 	movl	(%eax), %edx
 8048698:	8b 45 f8 	movl	-8(%ebp), %eax
 804869b:	83 c0 01 	addl	$1, %eax
 804869e:	8d 0c 85 00 00 00 00 	leal	(,%eax,4), %ecx
 80486a5:	8b 45 08 	movl	8(%ebp), %eax
 80486a8:	01 c8 	addl	%ecx, %eax
 80486aa:	8b 00 	movl	(%eax), %eax
 80486ac:	39 c2 	cmpl	%eax, %edx
 80486ae:	7e 50 	jle	80 <sortArray+0xa5>
 80486b0:	8b 45 f8 	movl	-8(%ebp), %eax
 80486b3:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 80486ba:	8b 45 08 	movl	8(%ebp), %eax
 80486bd:	01 d0 	addl	%edx, %eax
 80486bf:	8b 00 	movl	(%eax), %eax
 80486c1:	89 45 f4 	movl	%eax, -12(%ebp)
 80486c4:	8b 45 f8 	movl	-8(%ebp), %eax
 80486c7:	83 c0 01 	addl	$1, %eax
 80486ca:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 80486d1:	8b 45 08 	movl	8(%ebp), %eax
 80486d4:	01 d0 	addl	%edx, %eax
 80486d6:	8b 55 f8 	movl	-8(%ebp), %edx
 80486d9:	8d 0c 95 00 00 00 00 	leal	(,%edx,4), %ecx
 80486e0:	8b 55 08 	movl	8(%ebp), %edx
 80486e3:	01 ca 	addl	%ecx, %edx
 80486e5:	8b 00 	movl	(%eax), %eax
 80486e7:	89 02 	movl	%eax, (%edx)
 80486e9:	8b 45 f8 	movl	-8(%ebp), %eax
 80486ec:	83 c0 01 	addl	$1, %eax
 80486ef:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 80486f6:	8b 45 08 	movl	8(%ebp), %eax
 80486f9:	01 c2 	addl	%eax, %edx
 80486fb:	8b 45 f4 	movl	-12(%ebp), %eax
 80486fe:	89 02 	movl	%eax, (%edx)
 8048700:	83 45 f8 01 	addl	$1, -8(%ebp)
 8048704:	8b 45 0c 	movl	12(%ebp), %eax
 8048707:	83 e8 01 	subl	$1, %eax
 804870a:	39 45 f8 	cmpl	%eax, -8(%ebp)
 804870d:	0f 8c 74 ff ff ff 	jl	-140 <sortArray+0x2c>
 8048713:	83 45 fc 01 	addl	$1, -4(%ebp)
 8048717:	8b 45 fc 	movl	-4(%ebp), %eax
 804871a:	3b 45 0c 	cmpl	12(%ebp), %eax
 804871d:	0f 8c 5b ff ff ff 	jl	-165 <sortArray+0x23>
 8048723:	90 	nop
 8048724:	c9 	leave
 8048725:	c3 	retl
{% endhighlight %}

{% highlight asm %}
printArray: b4
 8048726:	55 	pushl	%ebp
 8048727:	89 e5 	movl	%esp, %ebp
 8048729:	53 	pushl	%ebx
 804872a:	83 ec 14 	subl	$20, %esp
 804872d:	e8 1e fd ff ff 	calll	-738 <__x86.get_pc_thunk.bx>
 8048732:	81 c3 ce 18 00 00 	addl	$6350, %ebx
 8048738:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 804873f:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 8048746:	eb 28 	jmp	40 <printArray+0x4a>
 8048748:	8b 45 f4 	movl	-12(%ebp), %eax
 804874b:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 8048752:	8b 45 08 	movl	8(%ebp), %eax
 8048755:	01 d0 	addl	%edx, %eax
 8048757:	8b 00 	movl	(%eax), %eax
 8048759:	83 ec 08 	subl	$8, %esp
 804875c:	50 	pushl	%eax
 804875d:	8d 83 b0 e8 ff ff 	leal	-5968(%ebx), %eax
 8048763:	50 	pushl	%eax
 8048764:	e8 17 fc ff ff 	calll	-1001 <printf@plt>
 8048769:	83 c4 10 	addl	$16, %esp
 804876c:	83 45 f4 01 	addl	$1, -12(%ebp)
 8048770:	8b 45 f4 	movl	-12(%ebp), %eax
 8048773:	3b 45 0c 	cmpl	12(%ebp), %eax
 8048776:	7c d0 	jl	-48 <printArray+0x22>
 8048778:	83 ec 0c 	subl	$12, %esp
 804877b:	6a 0a 	pushl	$10
 804877d:	e8 4e fc ff ff 	calll	-946 <putchar@plt>
 8048782:	83 c4 10 	addl	$16, %esp
 8048785:	90 	nop
 8048786:	8b 5d fc 	movl	-4(%ebp), %ebx
 8048789:	c9 	leave
 804878a:	c3 	retl
{% endhighlight %}

{% highlight asm %}
populateArray:
 8048516:	55 	pushl	%ebp
 8048517:	89 e5 	movl	%esp, %ebp
 8048519:	56 	pushl	%esi
 804851a:	53 	pushl	%ebx
 804851b:	83 ec 10 	subl	$16, %esp
 804851e:	e8 2d ff ff ff 	calll	-211 <__x86.get_pc_thunk.bx>
 8048523:	81 c3 dd 1a 00 00 	addl	$6877, %ebx
 8048529:	e8 b2 fe ff ff 	calll	-334 <rand@plt>
 804852e:	89 c1 	movl	%eax, %ecx
 8048530:	ba 1f 85 eb 51 	movl	$1374389535, %edx
 8048535:	89 c8 	movl	%ecx, %eax
 8048537:	f7 ea 	imull	%edx
 8048539:	c1 fa 05 	sarl	$5, %edx
 804853c:	89 c8 	movl	%ecx, %eax
 804853e:	c1 f8 1f 	sarl	$31, %eax
 8048541:	29 c2 	subl	%eax, %edx
 8048543:	89 d0 	movl	%edx, %eax
 8048545:	89 45 f0 	movl	%eax, -16(%ebp)
 8048548:	8b 45 f0 	movl	-16(%ebp), %eax
 804854b:	6b c0 64 	imull	$100, %eax, %eax
 804854e:	29 c1 	subl	%eax, %ecx
 8048550:	89 c8 	movl	%ecx, %eax
 8048552:	89 45 f0 	movl	%eax, -16(%ebp)
 8048555:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 804855c:	c7 45 ec 00 00 00 00 	movl	$0, -20(%ebp)
 8048563:	8b 45 f0 	movl	-16(%ebp), %eax
 8048566:	c1 e0 03 	shll	$3, %eax
 8048569:	83 ec 0c 	subl	$12, %esp
 804856c:	50 	pushl	%eax
 804856d:	e8 2e fe ff ff 	calll	-466 <malloc@plt>
 8048572:	83 c4 10 	addl	$16, %esp
 8048575:	89 c2 	movl	%eax, %edx
 8048577:	8b 45 08 	movl	8(%ebp), %eax
 804857a:	89 10 	movl	%edx, (%eax)
 804857c:	8b 45 f0 	movl	-16(%ebp), %eax
 804857f:	c1 e0 03 	shll	$3, %eax
 8048582:	83 ec 0c 	subl	$12, %esp
 8048585:	50 	pushl	%eax
 8048586:	e8 15 fe ff ff 	calll	-491 <malloc@plt>
 804858b:	83 c4 10 	addl	$16, %esp
 804858e:	89 45 e8 	movl	%eax, -24(%ebp)
 8048591:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 8048598:	eb 3c 	jmp	60 <populateArray+0xc0>
 804859a:	e8 41 fe ff ff 	calll	-447 <rand@plt>
 804859f:	89 c1 	movl	%eax, %ecx
 80485a1:	8b 45 f4 	movl	-12(%ebp), %eax
 80485a4:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 80485ab:	8b 45 e8 	movl	-24(%ebp), %eax
 80485ae:	8d 34 02 	leal	(%edx,%eax), %esi
 80485b1:	ba d3 4d 62 10 	movl	$274877907, %edx
 80485b6:	89 c8 	movl	%ecx, %eax
 80485b8:	f7 ea 	imull	%edx
 80485ba:	c1 fa 06 	sarl	$6, %edx
 80485bd:	89 c8 	movl	%ecx, %eax
 80485bf:	c1 f8 1f 	sarl	$31, %eax
 80485c2:	29 c2 	subl	%eax, %edx
 80485c4:	89 d0 	movl	%edx, %eax
 80485c6:	69 c0 e8 03 00 00 	imull	$1000, %eax, %eax
 80485cc:	29 c1 	subl	%eax, %ecx
 80485ce:	89 c8 	movl	%ecx, %eax
 80485d0:	89 06 	movl	%eax, (%esi)
 80485d2:	83 45 f4 01 	addl	$1, -12(%ebp)
 80485d6:	8b 45 f0 	movl	-16(%ebp), %eax
 80485d9:	01 c0 	addl	%eax, %eax
 80485db:	39 45 f4 	cmpl	%eax, -12(%ebp)
 80485de:	7c ba 	jl	-70 <populateArray+0x84>
 80485e0:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 80485e7:	eb 74 	jmp	116 <populateArray+0x147>
 80485e9:	e8 f2 fd ff ff 	calll	-526 <rand@plt>
 80485ee:	99 	cltd
 80485ef:	f7 7d f0 	idivl	-16(%ebp)
 80485f2:	89 d0 	movl	%edx, %eax
 80485f4:	01 c0 	addl	%eax, %eax
 80485f6:	89 45 ec 	movl	%eax, -20(%ebp)
 80485f9:	8b 45 ec 	movl	-20(%ebp), %eax
 80485fc:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 8048603:	8b 45 e8 	movl	-24(%ebp), %eax
 8048606:	01 d0 	addl	%edx, %eax
 8048608:	8b 00 	movl	(%eax), %eax
 804860a:	83 f8 ff 	cmpl	$-1, %eax
 804860d:	74 4e 	je	78 <populateArray+0x147>
 804860f:	8b 45 ec 	movl	-20(%ebp), %eax
 8048612:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 8048619:	8b 45 e8 	movl	-24(%ebp), %eax
 804861c:	8d 0c 02 	leal	(%edx,%eax), %ecx
 804861f:	8b 45 08 	movl	8(%ebp), %eax
 8048622:	8b 00 	movl	(%eax), %eax
 8048624:	8b 55 f4 	movl	-12(%ebp), %edx
 8048627:	c1 e2 03 	shll	$3, %edx
 804862a:	01 c2 	addl	%eax, %edx
 804862c:	8b 01 	movl	(%ecx), %eax
 804862e:	89 02 	movl	%eax, (%edx)
 8048630:	8b 45 08 	movl	8(%ebp), %eax
 8048633:	8b 00 	movl	(%eax), %eax
 8048635:	8b 55 f4 	movl	-12(%ebp), %edx
 8048638:	c1 e2 03 	shll	$3, %edx
 804863b:	01 d0 	addl	%edx, %eax
 804863d:	c7 40 04 00 00 00 00 	movl	$0, 4(%eax)
 8048644:	8b 45 ec 	movl	-20(%ebp), %eax
 8048647:	8d 14 85 00 00 00 00 	leal	(,%eax,4), %edx
 804864e:	8b 45 e8 	movl	-24(%ebp), %eax
 8048651:	01 d0 	addl	%edx, %eax
 8048653:	c7 00 ff ff ff ff 	movl	$4294967295, (%eax)
 8048659:	83 45 f4 01 	addl	$1, -12(%ebp)
 804865d:	8b 45 f4 	movl	-12(%ebp), %eax
 8048660:	3b 45 f0 	cmpl	-16(%ebp), %eax
 8048663:	7c 84 	jl	-124 <populateArray+0xd3>
 8048665:	8b 45 f0 	movl	-16(%ebp), %eax
 8048668:	8d 65 f8 	leal	-8(%ebp), %esp
 804866b:	5b 	popl	%ebx
 804866c:	5e 	popl	%esi
 804866d:	5d 	popl	%ebp
 804866e:	c3 	retl
{% endhighlight %}

{% highlight asm %}
sortArray:
 804866f:	55 	pushl	%ebp
 8048670:	89 e5 	movl	%esp, %ebp
 8048672:	83 ec 10 	subl	$16, %esp
 8048675:	e8 4f 02 00 00 	calll	591 <__x86.get_pc_thunk.ax>
 804867a:	05 86 19 00 00 	addl	$6534, %eax
 804867f:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 8048686:	c7 45 fc 00 00 00 00 	movl	$0, -4(%ebp)
 804868d:	e9 99 00 00 00 	jmp	153 <sortArray+0xbc>
 8048692:	c7 45 f8 00 00 00 00 	movl	$0, -8(%ebp)
 8048699:	eb 7d 	jmp	125 <sortArray+0xa9>
 804869b:	8b 45 f8 	movl	-8(%ebp), %eax
 804869e:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 80486a5:	8b 45 08 	movl	8(%ebp), %eax
 80486a8:	01 d0 	addl	%edx, %eax
 80486aa:	8b 10 	movl	(%eax), %edx
 80486ac:	8b 45 f8 	movl	-8(%ebp), %eax
 80486af:	83 c0 01 	addl	$1, %eax
 80486b2:	8d 0c c5 00 00 00 00 	leal	(,%eax,8), %ecx
 80486b9:	8b 45 08 	movl	8(%ebp), %eax
 80486bc:	01 c8 	addl	%ecx, %eax
 80486be:	8b 00 	movl	(%eax), %eax
 80486c0:	39 c2 	cmpl	%eax, %edx
 80486c2:	7e 50 	jle	80 <sortArray+0xa5>
 80486c4:	8b 45 f8 	movl	-8(%ebp), %eax
 80486c7:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 80486ce:	8b 45 08 	movl	8(%ebp), %eax
 80486d1:	01 d0 	addl	%edx, %eax
 80486d3:	8b 00 	movl	(%eax), %eax
 80486d5:	89 45 f4 	movl	%eax, -12(%ebp)
 80486d8:	8b 45 f8 	movl	-8(%ebp), %eax
 80486db:	83 c0 01 	addl	$1, %eax
 80486de:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 80486e5:	8b 45 08 	movl	8(%ebp), %eax
 80486e8:	01 d0 	addl	%edx, %eax
 80486ea:	8b 55 f8 	movl	-8(%ebp), %edx
 80486ed:	8d 0c d5 00 00 00 00 	leal	(,%edx,8), %ecx
 80486f4:	8b 55 08 	movl	8(%ebp), %edx
 80486f7:	01 ca 	addl	%ecx, %edx
 80486f9:	8b 00 	movl	(%eax), %eax
 80486fb:	89 02 	movl	%eax, (%edx)
 80486fd:	8b 45 f8 	movl	-8(%ebp), %eax
 8048700:	83 c0 01 	addl	$1, %eax
 8048703:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 804870a:	8b 45 08 	movl	8(%ebp), %eax
 804870d:	01 c2 	addl	%eax, %edx
 804870f:	8b 45 f4 	movl	-12(%ebp), %eax
 8048712:	89 02 	movl	%eax, (%edx)
 8048714:	83 45 f8 01 	addl	$1, -8(%ebp)
 8048718:	8b 45 0c 	movl	12(%ebp), %eax
 804871b:	83 e8 01 	subl	$1, %eax
 804871e:	39 45 f8 	cmpl	%eax, -8(%ebp)
 8048721:	0f 8c 74 ff ff ff 	jl	-140 <sortArray+0x2c>
 8048727:	83 45 fc 01 	addl	$1, -4(%ebp)
 804872b:	8b 45 fc 	movl	-4(%ebp), %eax
 804872e:	3b 45 0c 	cmpl	12(%ebp), %eax
 8048731:	0f 8c 5b ff ff ff 	jl	-165 <sortArray+0x23>
 8048737:	90 	nop
 8048738:	c9 	leave
 8048739:	c3 	retl
{% endhighlight %}

{% highlight asm %}
resetPrint:
 804873a:	55 	pushl	%ebp
 804873b:	89 e5 	movl	%esp, %ebp
 804873d:	83 ec 10 	subl	$16, %esp
 8048740:	e8 84 01 00 00 	calll	388 <__x86.get_pc_thunk.ax>
 8048745:	05 bb 18 00 00 	addl	$6331, %eax
 804874a:	c7 45 fc 00 00 00 00 	movl	$0, -4(%ebp)
 8048751:	c7 45 fc 00 00 00 00 	movl	$0, -4(%ebp)
 8048758:	eb 1a 	jmp	26 <resetPrint+0x3a>
 804875a:	8b 45 fc 	movl	-4(%ebp), %eax
 804875d:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 8048764:	8b 45 08 	movl	8(%ebp), %eax
 8048767:	01 d0 	addl	%edx, %eax
 8048769:	c7 40 04 00 00 00 00 	movl	$0, 4(%eax)
 8048770:	83 45 fc 01 	addl	$1, -4(%ebp)
 8048774:	8b 45 fc 	movl	-4(%ebp), %eax
 8048777:	3b 45 0c 	cmpl	12(%ebp), %eax
 804877a:	7c de 	jl	-34 <resetPrint+0x20>
 804877c:	90 	nop
 804877d:	c9 	leave
 804877e:	c3 	retl

{% endhighlight %}

{% highlight asm %}
printArray:
 804877f:	55 	pushl	%ebp
 8048780:	89 e5 	movl	%esp, %ebp
 8048782:	53 	pushl	%ebx
 8048783:	83 ec 14 	subl	$20, %esp
 8048786:	e8 c5 fc ff ff 	calll	-827 <__x86.get_pc_thunk.bx>
 804878b:	81 c3 75 18 00 00 	addl	$6261, %ebx
 8048791:	c7 45 f4 00 00 00 00 	movl	$0, -12(%ebp)
 8048798:	c7 45 f0 00 00 00 00 	movl	$0, -16(%ebp)
 804879f:	eb 60 	jmp	96 <printArray+0x82>
 80487a1:	e8 3a fc ff ff 	calll	-966 <rand@plt>
 80487a6:	99 	cltd
 80487a7:	f7 7d 0c 	idivl	12(%ebp)
 80487aa:	89 55 f0 	movl	%edx, -16(%ebp)
 80487ad:	8b 45 f4 	movl	-12(%ebp), %eax
 80487b0:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 80487b7:	8b 45 08 	movl	8(%ebp), %eax
 80487ba:	01 d0 	addl	%edx, %eax
 80487bc:	8b 40 04 	movl	4(%eax), %eax
 80487bf:	85 c0 	testl	%eax, %eax
 80487c1:	75 3e 	jne	62 <printArray+0x82>
 80487c3:	8b 45 f4 	movl	-12(%ebp), %eax
 80487c6:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 80487cd:	8b 45 08 	movl	8(%ebp), %eax
 80487d0:	01 d0 	addl	%edx, %eax
 80487d2:	8b 00 	movl	(%eax), %eax
 80487d4:	83 ec 08 	subl	$8, %esp
 80487d7:	50 	pushl	%eax
 80487d8:	8d 83 50 e9 ff ff 	leal	-5808(%ebx), %eax
 80487de:	50 	pushl	%eax
 80487df:	e8 9c fb ff ff 	calll	-1124 <printf@plt>
 80487e4:	83 c4 10 	addl	$16, %esp
 80487e7:	8b 45 f4 	movl	-12(%ebp), %eax
 80487ea:	8d 14 c5 00 00 00 00 	leal	(,%eax,8), %edx
 80487f1:	8b 45 08 	movl	8(%ebp), %eax
 80487f4:	01 d0 	addl	%edx, %eax
 80487f6:	c7 40 04 01 00 00 00 	movl	$1, 4(%eax)
 80487fd:	83 45 f4 01 	addl	$1, -12(%ebp)
 8048801:	8b 45 f4 	movl	-12(%ebp), %eax
 8048804:	3b 45 0c 	cmpl	12(%ebp), %eax
 8048807:	7c 98 	jl	-104 <printArray+0x22>
 8048809:	83 ec 0c 	subl	$12, %esp
 804880c:	6a 0a 	pushl	$10
 804880e:	e8 bd fb ff ff 	calll	-1091 <putchar@plt>
 8048813:	83 c4 10 	addl	$16, %esp
 8048816:	90 	nop
 8048817:	8b 5d fc 	movl	-4(%ebp), %ebx
 804881a:	c9 	leave
 804881b:	c3 	retl
{% endhighlight %}

