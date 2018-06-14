---
title: Virtual Memory(Draft)
date: 2018-06-12 20:48:04
tags: 
- Operating System
categories: Operating System
---
### Limitations of Prvious Memory Management Techniques

### Introduction to Virtual Memory
The basic idea is that each program has its own address space, which would be broken up into chunks called pages. In other words, a page is just a contiguous range of addresses that can be mapped onto physical memory. But not all pages have to be in memory at the same time

### Paging
pages are the the fixed-size unit of the virtual address space, and their corresponding units in the physical memory are called page frames. Normally, the size is 4KB, but larger page like 2MB, 1GB do exists. Transfers between RAM and disk are always in whole pages. When an instruction is executed, say
```
MOV REG, 0
```
the address 0, which is genereated by user application would be sent to Memory Management Unit(MMU), which would map it into real address in RAM. The mapping that MMU uses is stored in the page table. Once the instruction references an unmapped address, the MMU notices that and causes the CPU to trap to the operating system. This trap is called a page fault. The operating system picks a little-used page frame and writes its contents back to the disk (if it is not already there). It then fetches (also from the disk) the page that was just referenced into the page frame just freed, changes the map, and restarts the trapped instruction

### Page Table and Translation Lookaside Buffers
Conside a 1-byte instruction that copies one register to another. In the absence of paging, this instruction makes only one memory reference, to fetch the instruction. With paging, at least one additional memory reference will be needed, to access the page
table. Having to make two memory references per memory reference reduces performance by half.

The solution to the problem is to equip computers with a small hardware for mapping addresses without going through the page table, which is based on the assumption that most programs tend to make a large number of references to a small number of pages. This device is called Translation Lookaside Buffer(TLB) or associative memory. 

Normally, TLB is inside MMU and it can check for address tuples in parallel
