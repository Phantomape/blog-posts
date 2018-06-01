---
title: Intellisense in IDE
date: 2017-08-31 19:08:04
tags: 
- Intellisense
categories: Intellisense
---
##	Overview
During my internship this summer, our team wants to build a cloud IDE for other developers in the company and I was in charge of the autocompletion feature. It was absolutely a challenge for cause I have done anything related to it at all. So it took me a while to know how the whole system works. Due to security concern, I am not allowed to show you the code. However, this blog can still serve as an introduction and a basic tutorial for those who wants to equip their editor with intellisense.

Before I proceed to building our own cloud IDE, I did some researches on how IDE liek pycharm or eclipse provide intellisense; and I discovered that most of them make use of the compiler APIs, which provide rich information about the code snippet. In addition, with a more thorough dive into VS Code, I found out a handy tool called Microsoft Language Server Portocol. As I see it, instead of re-building our own interface to different compilers, why not make use of all kinds of existing language servers, which can save us the cost of maintainance. Accordingly, we decide to use Monaco as our editor, which is a core component of VS Code, and luckily, now all I have to do is to build a language client that can communicate with languages servers with jsonrpc.

##  To Be Continued
blah blah blah