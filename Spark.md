---
title: Introduction to Spark
date: 2017-08-28 19:08:04
tags: 
- Cloud Computing
categories: Cloud Computing
---
##	Overview
Spark that supports these applications while retaining the scalability and fault tolerance of
MapReduce. To achieve these goals, Spark introduces an abstraction called resilient distributed datasets (RDDs). An RDD is a read-only collection of objects partitioned across a set of machines that can be rebuilt if a partition is lost.
##	Traditional Systems
Traditional systems like Dryad and MapReduce s achieve their scalability and fault tolerance
by providing a programming model where the user creates acyclic data flow graphs to pass input data through a set of operators. However, they show limitation in cases as follows:

Iterative job:
>Many common machine learning algorithms apply a function repeatedly to the same dataset to optimize a parameter. While each iteration can be expressed as a MapReduce/Dryad job, each job must reload the data from disk, incurring a significant performance penalty.

Interactive analysis:
>Hadoop is often used to perform ad-hoc exploratory queries on big datasets, through SQL interfaces such as Pig and Hive. Ideally, a user would be able to load a dataset of interest into memory across a number of machines and query it repeatedly. However, with Hadoop, each query incurs significant latency (tens of seconds) because it runs as a separate MapReduce job and reads data from disk.
##	Programming Model
 Spark provides two main abstractions for parallel programming: resilient distributed data-sets and parallel operations on these datasets (invoked by passing a function to apply on a data-set). 

####	Resilient Distributed Datasets (RDDs)
A resilient distributed dataset (RDD) is a read-only collection of objects partitioned across a set of machines that can be rebuilt if a partition is lost. In Spark, each RDD is represented by a Scala object. Spark lets programmers construct RDDs in four ways:

1.	From a file in a shared file system, such as the Hadoop Distributed File System (HDFS).
2.	By “parallelizing” a Scala collection (e.g., an array) in the driver program, which means dividing it into a number of slices that will be sent to multiple nodes.
3.	By transforming an existing RDD. A dataset with elements of type A can be transformed into a dataset with elements of type B using an operation called flatMap, which passes each element through a user-provided function of type A ⇒ List[B].
4.	By changing the persistence of an existing RDD. By default, RDDs are lazy and ephemeral. That is, partitions of a dataset are materialized on demand when they are used in a parallel operation (e.g., by passing a block of a file through a map function), and are discarded from memory after use.However, a user can alter the persistence of an RDD through two actions:
>The $cache$ action leaves the dataset lazy, but hints that it should be kept in memory after the first time it is computed, because it will be reused.
>The $save$ action evaluates the dataset and writes it to a distributed filesystem such as HDFS. The saved version is used in future operations on it.

####	Parallel Operations
-	reduce:
Combines dataset elements using an associative
function to produce a result at the driver program
-	collect:
Sends all elements of the dataset to the driver
program
-	foreach:
Passes each element through a user provided
function.

####	Shared Variables
Programmers invoke operations like map, filter and reduceby passing closures (functions) to Spark. As is typical in functional programming, these closures can refer to variables in the scope where they are created. Normally, when Spark runs a closure on a worker node, these variables are copied to the worker. However, Spark also lets programmers create two restricted types of shared variables to support two simple but common usage patterns:

Broadcast variables: 
>If a large read-only piece of data (e.g., a lookup table) is used in multiple parallel operations, it is preferable to distribute it to the workers only once instead of packaging it with every closure.

Accumulators: 
>These are variables that workers can only “add” to using an associative operation, and that only the driver can read. They can be used to implement counters as in MapReduce and to provide a more imperative syntax for parallel sums. Accumulators can be defined for any type that has an “add” operation and a “zero” value. Due to their “add-only” semantics, they are easy to make fault-tolerant.
##	Implementation
Spark is built on top of Mesos and the core of Spark is the implementation of resilient distributed
datasets, which will be stored as a chain of objects capturing the lineage of each RDD
![RDD](http://img.blog.csdn.net/20161206044805792)

When a parallel operation is invoked on a dataset, Spark creates a task to process each partition of the dataset and sends these tasks to worker nodes. Finally, shipping tasks to workers requires shipping closures to them—both the closures used to define a distributed dataset, and closures passed to operations. The two types of shared variables in Spark, broadcast variables and accumulators, are implemented using classes with custom serialization formats. HDFS is used to broadcast variables, while accumulators are implemented using a different “serialization trick.”