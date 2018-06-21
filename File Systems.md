---
title: File Systems(Draft)
date: 2018-06-18 20:48:04
tags: 
- Operating System
categories: Operating System
---
#   Files

##  Inode
A data structure in a Unix-style file system which describes a filesystem object such as a file or a directory.(basic unit of meta data)

##  File System Types
There are several kinds of file systems, older versions of systems use FAT,while newer systems version like Windows 8 use NTFS

##  File Structure
There are three common structures for a file: byte sequence, record sequence and tree

##  File Types
*   Directories are actually system files for maintaining the structure of the whole file system. 
*   Regular files contain user information and are generally either ASCII files or binary files. Normally, a binary file from early UNIX system consists of 5 sections: header, text, data, relocationi bits and symbol table. The header will start with a so-called magic number, which would identify the file as an executable file. Then comes the sizes of various pieces of the file and etc. Another example is archives, which I don't know much about it.

##  File Attributes
aka metadata, password is also one of the attributes.

##  File Descriptors
small integers returned when a file is opened.

#   Directories
directories are also known as folders and they are used to keep track of files.

##  Absolute Path
The first character of the path name is the separator(in UNIX, it is '/')

##  Relative Path
Used in conjunction with the concept of the working directory

##  Link
### Hard Link
Can't cross disk boundaries but efficient. A technique that allows a file to appear in more than one directory. It would increment the counter in the file’s i-node (to keep track of the number of directory entries containing the
file)

### Symbolic Link
Can cross disk boundaries but less efficient. Instead of having two names point to the same internal data structure representing a file, a name can be created that points to a tiny file naming another file