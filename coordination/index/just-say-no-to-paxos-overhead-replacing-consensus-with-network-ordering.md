---
description: 'https://www.usenix.org/system/files/conference/osdi16/osdi16-li.pdf'
---

# Just Say NO to Paxos Overhead: Replacing Consensus with Network Ordering

### TL;DR 

The paper presents a new synchronization protocol, NOPaxos, which build on top of an asynchronous, unreliable network that supports ordered multicast. Because it assumes the underlying network provides ordering guarantees, NOPaxos outperforms the existing solution in terms of complexity and latency.

### Summary:

To guard against machine failures, modern systems store multiple replicas of the same data within and across data center. To ensure strong consistency, they need to implement some kinds of consensus protocol, like Paxos. However, existing works, which were built of top of an asynchronous, unreliable and unordered network, are too expensive. The insight of this paper is that if we could somehow implement a totally ordered atomic broadcast primitive, the consensus problem will be much easier.

The paper presents a novel idea of implementing consensus, which require cooperations between network layer and application layer. Specifically, the network layer is responsible for ensuring all receivers process the messages inn the same order, but message could be dropped.\(ordered unreliable multicast\(OUM\)\). Then, the job of application layer is to provide linearizability of client requests.\(network-ordered Paxos\(NOPaxos\)\)

**Ordered Unreliable Multicast**: It ensures an asynchronous, unreliable network that supports ordered multicast with drop detection.\[1\] It is enabled by the sequencer, which is a low-latency device that adds sequence number to each packets. The sequencer enables higher level application to discard messages that are received out of order and detect and report dropped messages by noticing gaps in the sequence number.

**Network-ordered Paxos**: Since NOPaxos is built on top of the guarantees of OUM network primitive, replicas one have to agree on which requests to execute and which to permanently ignore. When replicas receive a DROP-NOTIFICATION from libOUM, they first try to recover the missing request from each other. If that fails, the leader will initiates a round of agreement to commit a NO-OP.\[2\]

\[1\] If some message, m, is multicast to some set of processes, R, then either: \(1\) every process in R receives m or a notification that there was a dropped message before receiving the next multicast, or \(2\) no process in R receives m or a dropped message notification for m. 

\[2\] Non-leader replicas do this by contacting the leader for a copy of the request. If the leader itself receives a DROP-NOTIFICATION, it coordinates to commit a NO-OP operation in place of that request

