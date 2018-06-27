---
title: Deadlock(Draft)
date: 2018-06-26 19:08:04
tags: 
- Operating System
categories: Operating System
---
#   Resources
*   Preemptable resource: one that can be taken away from the process owning it with no ill effects, such as memory
*   Non-preemptable resource: one that cannot be taken away from its current owner without potentially causing failure, such as Blu-ray recorder

#   Conditions for Reource Deadlocks
1.  Mutual exclusion condition. Each resource is either currently assigned to exactly one process or is available.
2.  Hold-and-wait condition. Processes currently holding resources that were granted earlier can request new resources.
3.  No-preemption condition. Resources previously granted cannot be forcibly taken away from a process. They must be explicitly released by the process holding them
4.  Circular wait condition. There must be a circular list of two or more processes, each of which is waiting for a resource held by the next member of the chain.
