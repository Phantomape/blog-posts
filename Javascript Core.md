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

#   Execution Context
Honestly, I still don't know how to describe what context is. But there is something that I learnt from this article. The ```yield``` statement returns the value to the caller and pops the context while the ```next``` statement push the context onto stack and resume function.

#   Environment
So every execution context has an associated lexical environment, which is a structure used to define association between identifiers in the context. In that case, an environment has variables, functions and classes defined in it. We could think of it as a pair, consisting of an environment record (an actual storage table which maps identifiers to values), and a reference to the parent (which can be null).

The keyword ```var``` and ```with``` would provide the object as binding objects(new concept, never heard of it LOL). The binding object of the global environment is the global object and equals to ```this```.

Binding object can store a name which is not added to the environment record, since it's not a valid identifier:
```
this['not valid ID'] = 30;
```

#   Function
First take a look at two definitions from the article:

*   First-class function: a function which can participate as a normal data: be stored in a variable, passed as an argument, or returned as a value from another function.
*   Free variable: a variable which is neither a parameter, nor a local variable of this function.

For the following code, the ```x``` is a free variable and the program would resolve its binding from the outer scope where the function was created instead of from the caller scope, from where the function is called(dynamic scoping). That's why it is 10 intstead of 20. 

To implement lexical scoping, we need to capture the environment where the function is created, so think of it as every function has a reference to the environment.

#   Closure
If you took a loot at the last section, you can understand closure better
*   Closure: A closure is a function which captures the environment where it’s defined. Further this environment is used for identifier resolution.

#   This
Unlike other variables that are static scoped, ```this``` value is dynamically scoped and it can not mutate. The purpose of it is to executed the same code for multiple objects(think about clss-based OOP). For example, we are unable to determine ```this``` value by looking at the following code:
```
function foo() {
  return this;
}
```
But there is an exception, the arrow functions are special in terms of this value: their this is lexical (static). Holy shit, I can't understand the example code from the article

#   Realm
I have never heard of realm before, based on this article, before code is evalutated, all code must be associated with a realm, which provides a global environment for a context.

*   Realm: A code realm is an object which encapsulates a separate global environment.

#   Job
*   Job: A job is an abstract operation that initiates an ECMAScript computation when no other ECMAScript computation is currently in progress.

Jobs are enqueued on the job queues, and in current spec version there are two job queues: ScriptJobs, and PromiseJobs. 

And initial job on the ScriptJobs queue is the main entry point to our program — initial script which is loaded and evaluated: a realm is created, a global context is created and is associated with this realm, it’s pushed onto the stack, and the global code is executed.

#   Agent
*   Agent: An agent is an abstraction encapsulating execution context stack, set of job queues, and code realms.

The agents are state isolated from each other, and can communicate by sending messages. Some data can be shared though between agents, for example SharedArrayBuffers. Agents can also combine into agent clusters.