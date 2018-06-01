---
title: Global Interpreter Lock(GIL)
date: 2018-05-26 21:15:05
tags: 
- Python
categories: Python
---
Python is a very handy tool in terms of web development, machine learning and many other areas; however, it is also well-known for its poor performance in parallell computing due to global interpreter lock(GIL), which only exists in Cython. As I see it, its use of GIL doesn't bother me a lot, but it is still very intriguing and worth some time to investigate why lies beneath the GIL. I came across this [slide](http://www.dabeaz.com/python/GIL.pdf) on the train to Cupertino. 

##	Performance Experience
Let's consider a simple function
```
def count(n):    
	while n > 0:        
		n -= 1
```
compare the result of running it with and without threads. It turns out that the one with threads are far more slower than the other one. The result is based on a multi-core CPU and it you disable one of the CPU cores, the performance for the threaded program becomes slightly better.

##	Thread Creation
When you create a thread, python would create a small data structure containing some interpreter state and fires off a thread and call the function PyEval_CallObject.

##	Thread Execution
The interpreter has a global variable that points to the thread state structure of the current thread. So in conclusion, only one thread can execute in the interpreter at a time.

##	GIL Behavior
Threads hold the GIL when running; however, they would relaese it when blocked for I/O. When it deals with CPU-bound threads like our function, the interpreter periodically performs a "check" every 100 interpreter "ticks" by default. So what happens during the periodic check? There are many two things:
1. In the main thread only, signal handlers will execute if there are any pending signals (more shortly) 
2. Release and reacquire the GIL
```
// ceval.c
if (--_Py_Ticker < 0) {
    if (*next_instr == SETUP_FINALLY) {
        /* Make the last opcode before a try: finally: block uninterruptible. */
        goto fast_next_opcode;
    }
    _Py_Ticker = _Py_CheckInterval;
    tstate->tick_counter++;
    ....... /*for pending*/
}
if (interpreter_lock) {
    /* Give another thread a chance */

    if (PyThreadState_Swap(NULL) != tstate)
        Py_FatalError("ceval: tstate mix-up");
    PyThread_release_lock(interpreter_lock);

    /* Other threads may run now */

    PyThread_acquire_lock(interpreter_lock, 1);
}
```
If a signal arrives, the interpreter runs the "check" after every tick until the main thread runs. Since signal handlers can only run in the main thread, the interpreter quickly acquires/releases the GIL after every tick until it gets scheduled, which is why it is that slow in threaded program. This is partly why signals get so weird (the interpreter has no control over scheduling so it just attempts to thread switch as fast as possible with the hope that main will run).

##	Priority Inversion in Multicore Processor
A CPU-bound thread (low priority) can be blocking the execution of an I/O-bound thread (high priority). It occurs because the I/O thread can't wake up fast enough to acquire the GIL before the CPU-bound thread reacquires it.
