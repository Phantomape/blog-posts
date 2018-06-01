---
title: Computational Aspects of Robotics
date: 2017-08-28 19:08:04
tags: 
- Robotics
categories: Robotics
---
#   Course Notes on COMS W4733

textbook: Introduction to autonomous mobile robots 2nd edition
course website: www.cs.columbia.edu/~allen/F17/about.html

No midterm

##  Locomotion
Owing to limitations, mobile robots generally locomote either using wheeled
mechanisms.

### Key Issues for Locomotion
- stability
- characteristics of contact
- type of environment

### Legged Mobile Robots
Characterized by a series of point contacts between the robot and the ground. For a mobile robot
with legs, the total number of distinct event sequences for a walking machine is
$$N = (2k - 1)!$$
####	Advantages
* adaptability and maneuverability in rough terrain
* capable of crossing a hole or chasm so long as its reach
exceeds the width of the hole

####	Disadvantages
* power and mechanical complexity
* high maneuverability will only be achieved if the legs have a sufficient number of degrees of freedom to impart forces in a number of different directions

### Wheeled Mobile Robots

####	Advantages
* good efficiency
* no balance issues

####	Disadvantages
* traction
* stability
* maneuverability

### Aerial Mobile Robots

(I guess I need to post some photos in the future)
