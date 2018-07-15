---
title: Regex Engine Implementation(Draft)
date: 2018-07-15 20:48:04
tags: 
- Regex
categories: Regex
---
#   Introduction
This blog posts originate from a leetcode probelm: wildcard matching. By that time, I know very little about regex, so solving the problem brings me unspeakable frustration ：(. However, after graduation, I take a closer look into the concept and implementation of regex in my spare time and this blog mainly records what I have discovered so far.

#   Traditional Backtracking Approach
I have the implementation in my toy [repo](https://github.com/Phantomape/regex-engine)

#   Dynamic Programming Approach
This approach is mainly from leetcode, working on a limited set of regex meta-characters like ```.``` and ```*```.

#   Finite Automata Approach
I came across this approach in Russ Cox's article: [Regular Expression Matching Can Be Simple And Fast](https://swtch.com/%7Ersc/regexp/regexp1.html). To begin with, we need to know what finite automata is.

##  DFA and NFA
Finite automata are also known as state machines, which can be represented in the diagram by circles. Each character is modeled as a state(represented as circle) and they are linked directionaly.

If each possible input character leads to at most one new state, the whole machine(think of it as the set of all states and the execution process) is called a deterministic finite automation(DFA). The counterpart of DFA is non-deterministic finite automata(NFA or NDFA).

##  Converting Regex to NFAs
These two things turn out to be exactly equivalent in power: they can be converted to each other. Multiple ways exist for doing the conversion, but in here we'll only describe the method used in the paper, which is Thompson's approach. It is pretty easy to understand from the paper, where each NFA has no matching state but a dangling arrow.

##  Implementation
First, we need to declare an NFA:
```
// c < 256: one character
// c = 256: split
// c = 257: matched
struct State {
    int c;
    State *out;
    State *out1;
    int lastlist;
}
```