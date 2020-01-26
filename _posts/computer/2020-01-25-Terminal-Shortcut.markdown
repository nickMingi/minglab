---
layout: "post"
title: "Terminal shorcut compilation"
author: "mingi hong"
date: 2020-01-25 12:06:55 -0700
categories: Computer Knowledge
permalink: /:categories:/title.html
---

Terminal Shortcut

Mono 경로를 찾지 못해서 경로를 등록해줌

mdfind -> 원하는 파일 경로 찾기
 find /Users -name -> 원하는 파일 경로 찾기
echo $PATH - 현재 등록되어 있는 패스보기
export PATH=$PATH:/Library/Frameworks/Mono.framework/Versions/Current/bin/

현재설치 경로확인
which

현재경로 폴더열기
open .

파일 통합 
cat *.csv > merged.csv

1,2,4 번 column Delete
cut -d’,’ -f1-2,4- data.csv

1,2,5 번만 남기고 다 지움
cut -d, -f 1,2,5 merged.csv

1,2,5 번만 카피해서 outfield.csv로 이동
cut -d, -f 1,2,5 merged.csv> outfield.csv

* Google search keywords: 
    * linux bash …whatever from below…  
*   
* Linux Shell commands & scripting Overview/Outline 
    * Know the following: 
        * shell commands: 
            * which shell are we using? bash (default in linux mint)  
            * ls, pwd, mkdir, cp, mv, rm, cd  
            * cat, more, head, tail  
            * command options (switches) using - 
                * ls -a, ls -l  
            * vi, nano  
            * grep  
            * using the --help option on most commands  
            * redirection of I/O:  <   >   >>   2>  
            * using pipes "|" with linux shell command  
            * pause active process in terminal: ^z  
            * put process into background: ^z, bg 
                * gedit myfile.txt  
                * ^z  
                * bg  
            * run a process in background:  & 
                * ex: gedit myfile.txt &  
            * man  
            * view/manage processes:  ps, top, kill  
            * change file permissions:  chmod  
        * useful control codes in the consolse/terminal: 
            * ^Z (pause the running foreground process)  
            * ^D ("simulated" end of file)  
            * ^C "break" -- attemp to halt running process  
        * path names: 
            * linux file system: 
                * always rooted at "/"  
                    * "/" is the "root" directory  
                    * no "C:" silliness  
                * additional drives/partitions are "mounted" as folders within a single file system hierarchy  
        * linux scipting (bash scripting) 
            * text files 
                * #!/bin/bash   (that's always the first line of a bash shell script)q  
                * Make executable with chmod: 
                    * chmod +x myscript  
            * any bash commands you can run on the command line 
                * there are looping and "if" types of commands 
                    * -- google see what you can find!  
                    * google: bash shell scripting 
                        *  iteration   
                        * conditional   
                        * variables   
            * pass command line parameters:  $1 $2 $3…..  
                * experiment:  does $0 mean anything inside a script?  what??  
            * variables: 
                * $whatever  
                * Predefined variables: 
                    * $path 
                        * list of folders where bash will look for commands  
            * startup scripts 
                * .profile  
                    * setup/change user environment on login  
                * .bashrc  
                    * setup environment before bash runs 
                        * such as before running a script  

Bash Keyboard Shortcuts

Moving the cursor:
  Ctrl + a   Go to the beginning of the line (Home)
  Ctrl + e   Go to the End of the line (End)
  Ctrl + p   Previous command (Up arrow)
  Ctrl + n   Next command (Down arrow)
   Alt + b   Back (left) one word
   Alt + f   Forward (right) one word
  Ctrl + f   Forward one character
  Ctrl + b   Backward one character
  Ctrl + xx  Toggle between the start of line and current cursor position

Editing:
 Ctrl + L   Clear the Screen, similar to the clear command
 Ctrl + u   Cut/delete the line before the cursor position.
  Alt + Del Delete the Word before the cursor.
  Alt + d   Delete the Word after the cursor.
 Ctrl + d   Delete character under the cursor
 Ctrl + h   Delete character before the cursor (backspace)
 Ctrl + w   Cut the Word before the cursor to the clipboard.
 Ctrl + k   Cut the Line after the cursor to the clipboard.
  Alt + t   Swap current word with previous
 Ctrl + t   Swap the last two characters before the cursor (typo).
 Esc  + t   Swap the last two words before the cursor.
 Ctrl + y   Paste the last thing to be cut (yank)
  Alt + u   UPPER capitalize every character from the cursor to the end of the current word.
  Alt + l   Lower the case of every character from the cursor to the end of the current word.
  Alt + c   Capitalize the character under the cursor and move to the end of the word.
  Alt + r   Cancel the changes and put back the line as it was in the history (revert).
 Ctrl + _   Undo

 TAB        Tab completion for file/directory names
For example, to move to a directory 'sample1'; Type cd sam ; then press TAB and ENTER. 
type just enough characters to uniquely identify the directory you wish to open.

History:
  Ctrl + r   Recall the last command including the specified character(s)
             searches the command history as you type.
             Equivalent to : vim ~/.bash_history. 
  Ctrl + p   Previous command in history (i.e. walk back through the command history)
  Ctrl + n   Next command in history (i.e. walk forward through the command history)
   Alt + .   Use the last word of the previous command
  Ctrl + s   Go back to the next most recent command.
            (beware to not execute it from a terminal because this will also launch its XOFF).
  Ctrl + o   Execute the command found via Ctrl+r or Ctrl+s
  Ctrl + g   Escape from history searching mode

Process control:
 Ctrl + C   Interrupt/Kill whatever you are running (SIGINT)
 Ctrl + l   Clear the screen
 Ctrl + s   Stop output to the screen (for long running verbose commands)
 Ctrl + q   Allow output to the screen (if previously stopped using command above)
 Ctrl + D   Send an EOF marker, unless disabled by an option, this will close the current shell (EXIT)
 Ctrl + Z   Send the signal SIGTSTP to the current task, which suspends it.
            To return to it later enter fg 'process name' (foreground).

Emacs mode vs Vi Mode    
All the above assume that bash is running in the default Emacs setting, if you prefer this can be switched to Vi shortcuts instead.

Set Vi Mode in bash:
$ set -o vi

Set Emacs Mode in bash:    
$ set -o emacs
