---
title: File System Implementation(Draft)
date: 2018-06-20 19:08:04
tags: 
- Operating System
categories: Operating System
---
#   Layout
Most disks can be divided up into one or more partitions with independent file systems on each partition.

#   Implementation of Files
*   Contiguous Allocation
*   Linked-List Allocation
*   Linked-List Allocation Using a Table in Memory
*   I-node(index-node)

#   Implementation of Directories
When we want to open a file, the OS uses the path name provided by users to locate the directory entry on the disk, which provides the information to find the disk blocks. In general, directories are served to map the ASCII name of the file onto some info which is necessary for finding the data.

#   Shared Files
Let's assume A has a file to share with B. If directories of B contains disk addresses, then a copy of the addresses will have to be made in B's directory. If the file is appended, the new blocks appended will be listed only in the directory of the user doing the append.

There are two solutions to this problem: one is callled i-node(UNIX) or symbolic link.

#   Log Structure File Systems
*   Structure the whole disk as a log
*   Since i-nodes' address can not simply be calculated, an i-node map is maintained.
*   All writes are initially buffered in
memory, and periodically all the buffered writes are written to the disk in a single segment, at the end of the log.
*   A cleaner thread that spends its time scanning the log circularly to compact it.
*   Like a circular buffer
*   Measurements given in the papers cited above show that LFS outperforms UNIX by an order of magnitude on small writes, while having a performance that is as good as or better than UNIX for reads and large writes.

#   Journaling File Systems
The basic idea here is to keep a log of what the file system is going to do before it does it, so that if the system crashes before it can do its planned work, upon rebooting the system can look in the log to see what was going on at the time of the crash and finish the job.

#   Virtual File System
The virtual file system(VFS) has an upper interface to user processes and it is the well-known POSIX interface. Then comes the VFS interface