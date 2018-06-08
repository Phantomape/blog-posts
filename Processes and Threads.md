---
title: Processes and Threads
date: 2018-06-07 17:04:32
tags: 
- Operating System
categories: Operating System
---
##   Process
In UNIX, there is only one system call to create a new process: fork . This call
creates an exact clone of the calling process. After the fork , the two processes, the
parent and the child, have the same memory image, the same environment strings,
and the same open files.

In UNIX, a process and all of its children and further descendants together
form a process group. When a user sends a signal from the keyboard, the signal is
delivered to all members of the process group currently associated with the
keyboard

In Unix, all the processes in the whole system be-
long to a single tree, with init at the root. In contrast, Windows has no concept of a process hierarchy. All processes are
equal.

## Thread
Reasons for having threads:
*   in many
applications, multiple activities are going on at once. Some of these may block
from time to time. By decomposing such an application into multiple sequential
threads that run in quasi-parallel, the programming model becomes simpler.(It is very verbose!)
*   they are lighter weight than
processes, they are easier (i.e., faster) to create and destroy than processes.
*   Threads
yield no performance gain when all of them are CPU bound, but when there is sub-
stantial computing and also substantial I/O, having threads allows these activities
to overlap, thus speeding up the application.
