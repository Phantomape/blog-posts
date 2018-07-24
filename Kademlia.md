---
title: Kademlia(Draft)
date: 2018-07-24 20:48:04
tags: 
- Network
categories: Network
---
#   Introduction
Kademlia is the name of a peer-to-peer distributed hash table(DHT). The reason why I write this blog is because I'm currently writing a bittorrent client in golang and using the DHT to discover peers is the next step of the client implementation. [Here](https://zhuanlan.zhihu.com/p/40286711) is a tutorial writen in chinese, which is quite straightforward compared with the paper.

#   System Description
Like other DHTs(I know nothing about them LOL). a 160-bit opaque ID is assigned to each node and XOR is applied in calculating distance between nodes. A binary tree representation is used to represent the whole space, which can speed up the lookup process of any given node with constant time. Each node know at least one node in each subtree and the subtree is determined using a prefix. The highest subtree consist of the half of the binary tree not containing the node, then the next subtree consists of the remaining subtree not containing the node, and so forth.

#   K-Bucket

Each node has a list of k-buckets, and each k-bucket stores ```<IP address, UDP port, Node ID>```s(the triples represent a node) of distance between $2^i$ and $2^{i+1}$ from itself. Each bucket is ordered by placing the most-recently seen node to the tail of the bucket. k, as a parameter, represents the bucket size, not the list size. The list size is the number of buckets, in this paper, it should be 160 cause the 160-bit node representation.

#   Kademlia Protocol
The protocol consists of four RPCs: ```PING```, ```STORE```, ```FIND_NODE```, and ```FIND_VALUE```.
*   ```PING```: probes a node to see if it is online
*   ```STORE```: instructs a node to store a ```<key,value>``` pair for later retrieval
*   ```FIND_NODE```: takes a 160-bit ID as an arguent and returns ```k``` closest node with ```<IP address, UDP port, Node ID>```
*   ```FIND_VALUE```: no idea what it does

#   Routing Table
The routing table is a binary tree of which the subtrees are k-buckets.