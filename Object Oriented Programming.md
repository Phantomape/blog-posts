---
title: Object Oriented Programming(Draft)
date: 2018-07-05 19:08:04
tags: 
- OOP
categories: Programming Language
---
This article is full of shit that I don't understand:(
#   Static Class Based Model
The class represents a formal abstract set of the generalized characteristics of an instance (the knowledge about objects). Characteristics of instances are: properties (object description) and methods (object activity).

#   Prototype Based Model
objects can independently store all their characteristics (properties, methods) and do not need the class.

##  Delegation
In contrast with the static class based implementation, in case of impossibility to respond the message, the conclusion is: the object at the moment does not have the requested characteristic, however to get the result is still possible if to try to analyze alternative prototype chain, or probably, the object will have such characteristic after a number of mutations.

##  Concatenative Model
Concatenative prototyping is to not use delegation but exact copy of a prototype at the moment of object creation

#   Duck Typing
identification of objects can be made not by their hierarchy and belonging to concrete type, but by a current set of characteristics.

#   Encapsulation
The basic purpose of the encapsulation, repeat, is an abstraction from the user of the auxiliary helper data and not a “way to secure object from hackers”. Encapsulating auxiliary helper (local) objects, we provide possibility for further behavior changes of the public-interface with a minimum of expenses, localizing and predicting places of these changes. And exactly this is the main encapsulation purpose.

#   Mixins
Mixins have been suggested as an alternative to multiple inheritance. These are independent elements which can be mixed with any objects, extending their functionality.