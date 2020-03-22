---
description: 'https://web.stanford.edu/~ouster/cgi-bin/papers/raft-atc14'
---

# In Search of an Understandable Consensus Algorithm

### TL;DR:

Raft is a consensus algorithm that is designed to be easy to understand. It's equivalent to Paxos in fault-tolerance and performance.

### Summary:

This paper introduces Raft, a consensus algorithm that is designed to be more understandable and practical than Paxos. It is similar to Paxos in efficiency and fault-tolerance\(both algorithms operate in non-Byzantine, asynchronous model\).

Before Raft, there already exist several consensus protocols\(e.g., Paxos, view-stamped replication\). However, as we know, Paxos is notoriously difficult to understand. Although the algorithm itself is not very complicated, the question I always ask when I study Paxos is: Why does it work? Besides, the Paxos algorithm introduced by Lamport is somewhat incomplete. For example, it doesn’t address the “Dueling Proposers” problem and the cluster membership management. The original version of Paxos\(called Synod\) is also inefficient since it needs two rounds to choose a single value.

The overall goal of Raft is same as multi-Paxos, which is to replicate a log of commands across a collection of computers. The techniques used by the authors include problem decomposition and reducing state space\(“handle multiple problems with single mechanism”\). The main difference between Paxos and Raft is that Raft will select one server to act as a leader and that server will be responsible for replicating the logs. The leader election of Raft is similar to the prepare phase of Paxos. Raft adds some restrictions on which server can be elected. Specifically, servers with incomplete logs are not eligible to become a leader.

After it elects a leader, the leader starts accepting command from the clients and appends the command to its log. Like the proposers in Paxos, the leader in Raft will send RPCs to all followers and declares victory when it gets the response from a majority of followers.

### Note: 

1. Raft uses Terms to identify obsolete information\(e.g., old leaders\). Each term starts with a leader election and has at most one leader\(some terms may have no leader\) 

2.I think the leader election in Raft is somewhat different from the leader election algorithm\(e.g., Bully algorithm\). The leader election in Raft guarantees that only one leader is elected per term, but may fail due to timeouts in multiple followers, which means it may violate termination. In contrast, the other leader election algorithm solves “weak agreement” where it may not satisfy agreement\(There might exist two leaders at the same time\), but it always terminates.

### Related Links:

{% embed url="https://www.youtube.com/watch?v=YbZ3zDzDnrw" %}



