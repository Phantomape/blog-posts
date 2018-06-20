---
title: File Systems(Draft)
date: 2018-06-18 20:48:04
tags: 
- Operating System
categories: Operating System
---
#   Files

##  File System Types
There are several kinds of file systems, older versions of systems use FAT,while newer systems version like Windows 8 use NTFS

##  File Structure
There are three common structures for a file: byte sequence, record sequence and tree

##  File Types
*   Directories are actually system files for maintaining the structure of the whole file system. 
*   Regular files contain user information and are generally either ASCII files or binary files. Normally, a binary file from early UNIX system consists of 5 sections: header, text, data, relocationi bits and symbol table. The header will start with a so-called magic number, which would identify the file as an executable file. Then comes the sizes of various pieces of the file and etc. Another example is archives, which I don't know much about it.

##  File Attributes
aka metadata