---
layout: "post"
title: "OperatingSystem CH3 Process"
date: 2020-01-23 12:00:59 -0700
author: "mingi hong"
categories: Computer OperatingSystem
permalink: /:categories/:title.html
---

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