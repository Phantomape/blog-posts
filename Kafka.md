---
title: Introduction to Kafka
date: 2017-08-28 19:08:04
tags: 
- Cloud Computing
categories: Cloud Computing
---
##	About Kafka
####	Overview
Kafka is a distributed messaging system for collecting and delivering high volumes of log data with low latency.  On the one hand, Kafka is distributed and scalable, and offers high throughput. On the other hand, Kafka provides an API similar to a messaging system and allows applications to consume log events in real time
####	Traditional Messaging System
Traditional enterprise messaging systems are not to be a good fit for log processing. The reason is as follows:
1.	There is a mismatch in features offered by enterprise systems like out of order and unneeded features
2.	Many systems do not focus as strongly on throughput as their primary design constraint like no batch operations
3.	Weak in distributed support
4.	Many messaging systems assume near immediate consumption of messages, so the queue of unconsumed messages is always fairly small.
####	Kafka 
 A stream of messages of a particular type is defined by a topic. To balance load, a topic is divided into multiple partitions and each broker stores one or more of those partitions. A producer can publish messages to a topic. The published messages are then stored at a set of servers called brokers. A consumer can subscribe to one or more topics from the brokers, and consume the subscribed messages by pulling data from the brokers
 ![Kafka Architecture](http://img.blog.csdn.net/20161206045920225)
#####	1.	Efficiency on Single Partition
-	Simple Storage: 
Every time a producer publishes a message to a partition, the broker simply appends the message to the last segment file. The segment files are written to disk only after a configurable number of messages have been published or a certain amount of time has elapsed. A message is only exposed to the consumers after it is flushed and a message stored in Kafka does not have an explicit message id. Instead, each message is addressed by its logical offset in the log.
![Kafka Log](http://img.blog.csdn.net/20161206050018554)
-	Efficient Transfer:  
Avoid explicitly caching messages in memory at the Kafka layer. Instead, cache them on the underlying file system page cache, which avoids double buffering.  Since Kafka does not cache messages in process at all, it has very little overhead in garbage collecting its memory, making efficient implementation in a VM-based language feasible. Plus, Kafka is a multi-subscriber system and a single message may be consumed multiple times by different consumer applications
-	Stateless Broker: 
The volume of info consumed is stored in consumer, which reduce complexity and the overhead on the broker. A message is automatically deleted if it has been retained in the broker longer than a certain period, typically 7 days; thus, A consumer can deliberately rewind back to an old offset and re-consume data. 
#####	2.	Distributed Coordination
-	A partition within a topic is the smallest unit of parallelism. Kafka has the concept of consumer groups. Each consumer group consists of one or more consumers that jointly consume a set of subscribed topics, and each message is delivered to only one of the consumers within the group. 
-	Not having a central "master" node, but instead let consumers coordinate among themselves in a decentralized fashion. To facilitate the coordination, we employ a highly available consensus service Zookeeper. Zookeeper has a very simple, file system like API and it has the following functions:
>	Zookeeper can create a path, set the value of a path, read the value of a path, delete a path, and list the children of a path. It does a few more interesting things: (a) one can register a watcher on a path and get notified when the children of a path or the value of a path has changed; (b) a path can be created as ephemeral (as oppose to persistent), which means that if the creating client is gone, the path is automatically removed by the Zookeeper server; (c) zookeeper replicates its data to multiple servers, which makes the data highly reliable and available. Kafka uses Zookeeper for the following tasks: (1) detecting the addition and the removal of brokers and consumers, (2) triggering a rebalance process in each consumer when the above events happen, and (3) maintaining the consumption relationship and keeping track of the consumed offset of each partition
#####	3.	Delivery Guarantees
-	In general, Kafka only guarantees at-least-once delivery
-	Kafka guarantees that messages from a single partition are delivered to a consumer in order

##	Install Kafka
Download and extract Kafka from http://kafka.apache.org/downloads.html. For me, I have unzipped it in C:\Kafka
##	Set Up Kafka
1. Go to your Kafka config directory. For me its C:\Kafka\kafka_2.11-0.10.1.0\config
2. Edit file "server.properties"
3. Find & edit line "log.dirs=/tmp/kafka-logs" to "log.dir= C:\Kafka\kafka_2.11-0.10.1.0\kafka-logs".(Actually it doesn't matter where you put your log)
4. If your Zookeeper is running on some other machine or cluster you can edit "zookeeper.connect:2181" to your custom IP and port. For this demo we are using same machine so no need to change. Also Kafka port & broker.id are configurable in this file. Leave other settings as it is.
5. Your Kafka will run on default port 9092 & connect to zookeeper’s default port which is 2181.
##	Run Kafka Server
1. Go to your Kafka installation directory C:\Kafka\kafka_2.11-0.10.1.0\
2. Open a command prompt here by pressing Shift + right click and choose "Open command window here" option)
3. Now type .\bin\windows\kafka-server-start.bat .\config\server.properties and press Enter.(Before you run Kafka server, make sure you have opened ZooKeeper)
![Kafka](http://img.blog.csdn.net/20161115055337719)