---
layout: "post"
title: "OperatingSystem CH3 Process"
date: 2020-01-23 12:00:59 -0700
author: "mingi hong"
categories: Computer OperatingSystem
permalink: /:categories/:title.html
---

## Chapter 2

operating systems provide an environment for execution of programs and services to programs and users

one set of operating-system services provides functions that are helpful to the user:
- User Interface: almost all operating systems have a user interface varies between CLI(command-line), GUI(Graphics user interface), Batch
- Program execution: The system must be able to load a program into memory and to run that program, end execution, either normally or abnormally(indicating error)
- I/O operation: a running program may require I/O, which may involve a file or an I/O device

OS service provides functions that are helpful to user
- File-system manipulation
- Communications
- Error detection

Efficient operation
- Resource allocation
- Accounting
- Protection and security
	- protection
	- security

# CLI
1. sometimes implemented in kernel, sometimes by systems program
2. sometimes multiple flavors implemented - shells
3. primarily fetches a command from user and executes it
4. sometimes commands built-in sometimes just names of programs
	- if the latter, adding new features doesn't require shell modification

# GUI
1. user-friendly desktop metaphor interface
	- usually mouse, keyboard, and monitor
	- icons represent files, programs, actions, etc
	- various mouse buttons over objects in the interface cause various actions(provide information, options, execute function, open directory(known as a folder))
	- invented at xerox parc
2. many systems now include both CLI and GUI interfaces
	- microsoft windows is GUI with CLI command shell
	- apple mac os x is "aqua" gui interface with unix kernel underneath and shells available
	- unix and linux have CLI with optional GUI interfaces(CDE,KDE,GNOME)

# Touchscreen interfaces
1. mouse not possible or not desired
2. actions and selection based on

# System calls
1. Programming interface to the services provided by the OS
2. Typically written in a high-level language(C or C++)
3. Mostly accessed by program via a high-level application Programming interface(API) rather than direct system call use
4. Three most common APIs are Win32 API for Windows, POSIX API for POSIX-based systems(including virtually all versions of UNIX, Linux and Mac OS X), and Java API for the Java virtual machine(JVM)

- note that the system-call names used throughout this text are generic

# System call parameter passing
1. often, more information is required than simply identity of desired system call
	- Exact type and amount of information vary according to OS and call
2. Three general methods used to pass parameters to the OS
	- simplest: pass the parameters in registers
	- parameters stored in a block, or table, in memory, and address of block passed as a parameter in a register
	- parameters placed, or pushed, onto the stack by the program and popped off the stack by the operating system
	- block and stack methods do not limit the number or length of parameters being passed

#Types of system calls 
- File management
- Device management
- Information maintenance
- Communications
- Protection

Processes: a program in execution

Heuristic Algorithm(Time constraint)

Batch: job

Concurrent: task

Process State:

Ready - running - waiting -> this cycle called MIX

The degree of multiprogramming(The number of process in the MIX)

Controlling the degree of Programming CPU utilization

# Purpose of OS

1. participate

![MIX_diagram](/minglab/assets/MIX_diagram.png)

# Process creation
1. PID = fork()

Parent process can create child processes

And Child also can create child processes

`Where a Process creates New Process`
1. The parent continues
2. The parent waits

`Address Space`
1. The child is a duplicate of the parent
2. The child has a new program loaded  

# Terminate
1. Parent is in control
    - parent can kill a child
    - go to terminate status -> leave the system
2. Casacading termination
    - kill whole family
3. Orphan
    - kill only parent
    - since they don't have parents, they goes to 'init' process as their parent
4. Zombie
    - who is in the process of dying

**Windows Example**

{% highlight c %}
#include <stdio.h>
#include <windows.h>

int main (void)
{
  STARTUPINFO si;
  PROCESS_INFORMATION pi;
  
   ZeroMemory(&si, sizeof(si));
   si.cb = sizeof(si);
   ZeroMemory(&pi,sizeof(pi));
   
   if (!CreateProcess(NULL,
        "C:\\WINDOWS\\system32\\mspaint.exe",
		NULL,
		NULL,
		FALSE,
		0,
		NULL,
		NULL,
		&si,
		&pi)
	  )
	{
		fprintf(stderr, "Create Process Failed");
		return -1;
	}		
	
	WaitForSingleObject(pi.hProcess, INFINITE);
	printf("Child Complete");
	
	CloseHandle(pi.hProcess);
	CloseHandle(pi.hThread);
		
	//return 0;
}
{% endhighlight %}

if you erase waiting, parent will do his work <font color='red'>not depends</font> on child's work.

{% highlight c %}
    //WaitForSingleObject(pi.hProcess, INFINITE);
	printf("Child Complete");
{% endhighlight %}

**Unix Example**

{% highlight c %}
#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>


// similar to Fig 3.9 of text

int main( void)
{
	pid_t pid;
	
	//pid = fork();  // fork a child process
	pid = fork();  // fork a child process
	
	if (pid < 0)
	{
		fprintf(stderr,"Fork Failed");
	    return -1;
    }
	else
		if (pid == 0) //child
		{
		  //while (1);  // SPIN
		  //execlp("/bin/ls","ls",NULL);
          //fprintf(stderr,"CHILD \n");
		  printf("Child \n");
		}
	else //parent
	{
		//wait(NULL);
		//fprintf(stderr,"PARENT \n");
		printf("Parent \n");
	}

	return 0;
}
{% endhighlight %}

Here, if you put `execlp("/bin/ls","ls",NULL);` children will be terminated so `printf("Child \n");` is not printed

{% highlight c %}
else
		if (pid == 0) //child
		{
		  //while (1);  // SPIN
		  execlp("/bin/ls","ls",NULL);
          //fprintf(stderr,"CHILD \n");
		  printf("Child \n");
		}
{% endhighlight %}

`Windows:
Child is becoming new stuff without duplicating`

`Unix:
Child is duplicating parent and becoming new stuff`

# Interprocess Communication (concurrency)

1. Independent Process
	- Not affect or is affected by other processes
2. Cooperating Process
	- If it can affect other processes
	- Shared Data

Reasons for cooperation
1. Information Sharing
2. Computational Speed-up(Need "parallel" execution)
3. Modularity
4. Convenience

# How do processes communicate

Processes require an interprocess communication method(IPC)
1. Shared Memory
	- Faster(Usually no system[after setup] calls)
2. Message Passing
	- Exchanging small amounts of data
	- Easier to implement in distributed systems
		(Usually done using system calls)

Note: Multiprocessor core system may have issues with shared memory because of 'casche coherency'


# Shared Memory
- Establish a register of shared memory
- Usually it is placed in the process that creates the shared space
- "No more OS support required"
- Responsibility to use "correctly" (User Processes)

# Message Passing
- The OS provides a message passing facility
- Communicate / Synchronize
	- No shared memory

# Direct/ Indirect Communication
- Direct: Each process must "explicitly" name the other processes
	- Send(P,msg)
	- Receive(Q,msg)
	- Properties
		- process only needs to know the identity of other(Link is setup automatically)
		- A link is setup with exactly two processes
		- Only one(1) link between a pair of processes
			- Symmetric in addressing(Know each other)
			- A symmetric(Only the sender nows)
				1. Send(P,msg)
				2. Recieve(id,msg)
- Direct leads to Head-Coded
- Indirect: Message sent and recieved using mailbox supported by port
	- Send(A,msg) ; A is mailbox
	- Receive(A,msg)
	- Properties
		- If process share a mailbox then there is a link between them
		- Can be used to link more than two(2) processes
		- A pair of processes could have more than one(1) link
	- Handling conflicts
		1. Allow only links of two processes
		2. Allow only 1 process at a time to execute a receive
		3. Allow the OS to select "arbitrarily"

# Who "owns" the mailbox
- If owned by process:
	- it is part of its address space
		- can only receive
	- It is owned by the OS
		- process has to be able to request a mailbox
		- send | receive
		- delete the mailbox

# Synchronization
- Send - msg passing
	- Blocking(synchronizing) send(sync)
		- The sender blocks until the message is recieved -> mailbox or process
	- Non Blocking (async)
- Receive()
	- Blocking
	- Non Blocking
- If both are blocking "Rendevous"
- Buffering : talk about implementation
	- Zero capacity: Length is zero
	- Bounded capacity: Finite Length N
	- Unbounded capacity: (sender never blocks)

# Socket

1. Should know what Remote procedure call
	- we have to know the socket that i send it to
	- Problem is many clients want to access server it is dynamic
2. Match maker
	- Usually in the server side

![matchmaker1](/minglab/assets/matchmaker1.png)
![matchmaker2](/minglab/assets/matchmaker2.png)


# Pipe IPC

1. One of the first IPC mechanisms in early UNIX
2. Pipe 
	- Unidirectional?
	- Half duplex or full duplex?
	- Is it local network?
	- One way(Ordinary pipe)
	- Named pipe (persistent so we can use it again don't need parent and child relationship

	