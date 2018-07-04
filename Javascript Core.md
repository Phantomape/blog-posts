---
title: Javascript Core(Draft)
date: 2018-07-02 19:08:04
tags: 
- Javascript
categories: Programming Language
---
This article is based on [Javascript Core](http://dmitrysoshnikov.com/ecmascript/javascript-the-core-2nd-edition/). 
#   Object and Prototype
Although js is known as "object-oriented" programming language, it is quite different from my understanding of OOP. Mostly because they didn't actually teach OOP in college :D.
First concept to take in mind is:

*    An object is a collection of properties, and has single prototype object. The prototype may be either an object or the null value. Every object when created, received its prototype, which is a delegation object used to implement prototypbe-based inheritance.

Based on the previous experience with C++, I think of it as the base class at first. In other words, for a prototype chain:```A->B->Object.prototype->null```. ```A``` is inherited from ```B``` and ```A``` has a reference to it, which is the dunder proto(__proto__), and so on.

When a property is not found in the object, we'll attempte to resolve it in the prototype. This mechanism is known as dynamic dispatch or delegation.

#   Class
The most astonishing part of that article comes next, which change my understanding of class. In js, class is more like a syntax sugar, consisting of a constructor function and prototype. The prototype is stored in the constructor function's prototype property. Creating a prototype and sets the prototype for newly created objects is what class constructor does. You could go to that article and see the corresponding code, which will definitely enlighten you. 