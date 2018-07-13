---
title: Two-phase Commit
date: 2018-07-13 20:48:04
tags: 
- Distributed System
categories: Distributed System
---
#   Introduction
Two-phase commit(2PC) is an atomic commitment protocol, which is related to but different from consensus, and is widely used to implemente distributed transactions.

#   Protocol
*   A coordinator sends a proposal to all participants.
*   When a participant receives a proposal, it responds with a "Yes" or "No". If the participants votes "No", it can immediately abort.
*   The coordinator collects votes, if all were "Yes", the coordinator decides to commit and sends "Commit" message to each participant; otherwise, it will send "Abort" message to each participant that voted "Yes"
*   Participants wait for message from the coordinator. When they receive one, they should act accordingly.

From my perspective, the protocol itself is already very similar to paxos.

#   Timeouts
The words from the ppt is so mysterious, I couldn't interpret it :(

#   Failover

#   Conclusion
*   2PC guarantees consistency but not availablity.