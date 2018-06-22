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
