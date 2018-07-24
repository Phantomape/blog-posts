---
title: Kademlia(Draft)
date: 2018-07-24 20:48:04
tags: 
- Network
categories: Network
---
#   Introduction
Kademlia is the name of a peer-to-peer distributed hash table(DHT). The reason why I write this blog is because I'm currently writing a bittorrent client in golang and using the DHT to discover peers is the next step of the client implementation.

#   System Description
Like other DHTs(I know nothing about them LOL). a 160-bit opaque ID is assigned to each node and XOR is applied in calculating distance between nodes. A binary tree representation is used to represent the whole space, which can speed up the lookup process of any given node with constant time. Each node know at least one node in each subtree and the subtree is determined using a prefix. The highest subtree consist of the half of the binary tree not containing the node, then the next subtree consists of the remaining subtree not containing the node, and so forth.