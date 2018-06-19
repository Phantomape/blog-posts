---
title: Segmentation(Draft)
date: 2018-06-18 20:48:04
tags: 
- Operating System
categories: Operating System
---
### Why Segmentation
Consider a compiler as an example, it has several tables as compilation proceeds, such as the symbol table, the parse tree table, the stack used for procedure calls and etc.;If one of the table is exceptionally large, we would want it to be managed dynamically, in other words, a way of freeing the programmer from having to manage the expanding and contracting tables. That's what segmentation is for.

### What is Segmentation
Segmentation is a linear sequence of addresses.
