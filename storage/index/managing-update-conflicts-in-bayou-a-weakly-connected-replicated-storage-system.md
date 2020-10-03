---
description: 'http://db.cs.berkeley.edu/cs286/papers/bayou-sosp1995.pdf'
---

# Managing Update Conflicts in Bayou, a Weakly Connected Replicated Storage System

### TL;DR:

Bayou is a replicated, weakly consistent storage system design for mobile devices, which means it needs to able to handle frequent disconnection/weak connectivity of the devices. Similar to Dynamo, Bayou is also an AP system and provides eventual consistency.

### Summary:

The problem they were trying to solve is that how to build a system that allows the clients do operations from any server, especially local server. Because of the weak connectivity available for mobile computing, concurrent updates are very common. Thus, how to deal with concurrent operations on same data is the most critical problem.

Bayou, like Dynamo, guarantees availability and low latency. However, unlike Dynamo, which is a partially replicated storage system, Bayou is implemented as an optimistically fully replicated system where all the servers have all the data. The ways that Bayou resolve conflicts are a bit different from what Dynamo does. Bayou lets the application to write the reconciliation logic. Precisely, one “update” in Bayou consists of three components:1. The update itself 2. A dependency check, which checks if there is a conflict according to application logic. 3. A merge function specified by application to resolve conflicts. Bayou establishes a total order over all the operations by their timestamp and re-executes some operations if needed.

The big difference between Bayou and other storage system is that there are two kinds of updates\(writes\): tentative updates and committed updates. The updates initially accepted by the server are called “tentative updates”. Bayou will choose a server as primary, responsible for committing the changes. The main advantages of having two phases are 1. They allow Bayou to provide session guarantees\(especially read my writes\), in addition to eventual consistency. 2. Bayou can still be available while the system to trying to achieve consensus\(use Paxos?\) about the order of writes.

### Weakness: 

1.Because Bayou provides the ability to redo updates, it’s tough to do garbage collection. 2.Because the client only needs to do reads and writes from one replica without coordination with other replicas, some updates may not survive server failures. Finally, although both Bayou and Dynamo implement an anti-entropy protocol to achieve eventual consistency, I think Dynamo’s Merkle tree-based anti-entropy protocol is more efficient. Merkle Tree based protocol reduces the amount of data needed to pass around and relies on the actual data instead of timestamps. 3. It only supports single data center but does not works well in geo-distributed data centers.

### Related Links:

{% embed url="https://www.youtube.com/watch?v=tg7zvyWm\_NE" %}



