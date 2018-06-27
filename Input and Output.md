---
title: Input and Output(Draft)
date: 2018-06-26 19:08:04
tags: 
- Operating System
categories: Operating System
---
#   I/O Devices
*   Block devices: stores information in fixed-size blocks, each one with its own address.
*   Character devices: delivers or accepts a stream of characters, without regard to any block structure.

I/O units often consist of a mechanical component and an electronic component. The electronic component is called the device controller or adapter. The mechanical component is the device itself.

Normally the controller would receive a serial bit stream starting with a preamble, then the 4096 bits in a sector, and finally a checksum, or ECC (Error-Correcting Code).

Its job is to convert the serial bit stream into a block of bytes and perform any error correction necessary.

#   Communication between CPU and Control Register
*   Assign control register an I/O port number
*   Memory-mapped I/O, which is to map all control registers into memory space by assigning a unique memory address. With it, the I/O device driver can be written entirely in C and no special protection mechanism is needed to keep user processes from performing I/O. However, a device control register should not be cached.