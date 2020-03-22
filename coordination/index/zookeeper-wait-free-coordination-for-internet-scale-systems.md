---
description: 'https://www.usenix.org/legacy/event/atc10/tech/full_papers/Hunt.pdf'
---

# ZooKeeper: Wait-free coordination for Internet-scale systems

### TL;DR:

ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services.

### Summary:

Zookeeper is a coordination\[1\] service for distributed systems. It provides to its clients the abstraction of a set of data nodes\(zNodes\), organized according to a hierarchical name space. A znode may act as both a file containing binary data and a directory with more znodes as children\[2\]. ZooKeeper supports the concept of ephemeral znodes and regular\[3\] znodes. An ephemeral znodes will be removed by the system automatically when the session that creates them terminates. An important feature of Zookeeper is that it allows client to receive timely notifications of changes without requiring polling.\[4\] The last important concept is sessions. A client connects to Zookeeper and initiates a session. Sessions have an associated timeout.

Although ZooKeeper provides both distributed storage and message queue, you will run into some issue if you have Zookeeper for everything. First, there is a size limit of each znode\(~1MB\). Thus, in practice, znodes are used for metadata or configuration in a distributed computation. Similarly, you are likely to end up with throughput issues if you use Zookeeper as a message queue.

Zookeeper has two basic ordering guarantees: Linearizable writes and FIFO client order\[5\]. ZooKeeper process read requests locally at each replica. This allows the service to scale linearly as servers are added to the system.

### Implementation:\(See Figure 4\)

* ZooKeeper provides high availability by replicating the ZooKeeper data on each server that composes the service. The replicated database is an in-memory database containing the entire data tree.\(Each znode stores a maximum of 1MB of data by default\). It will keep a replay log\(a write-ahead log\) of committed operations and generate periodic snapshots of the in-memory database. 
* Write requests are processed by an agreement protocol. All write requests are handled by the leader\[6\]. The rest of the Zookeeper servers are followers.
* Request Processor: When the leader receives a write request, it calculates what the state of the system will be when the write is applied and transforms it into a transaction that captures this new state. 
* Atomic Broadcast: The leader executes the request and broadcasts the change to the Zookeeper state through Zab\[7\]. Zookeeper can only work if a majority of servers are correct. Zab guarantees that changes broadcast by a leader are delivered in the order they were sent and all changes from previous leaders are delivered to an established leader before it broadcasts its own changes. In normal operation, Zab does deliver all message in order and exactly once, but may redeliver a message during recovery. 
* Replicated database: Each replica has a copy in memory of the ZooKeeper state. For high performance, Zookeeper use periodic snapshots and only requires redelivery of messages since the start of the snapshot. Zookeeper uses fuzzy snapshots since it doesn't require any locks. The snapshot may not correspond to the sate of Zookeeper at any point in time. 
* Client-server Interaction: One drawback of handling reads locally is that a read operation may return a stale value. To guarantee that a give read operation returns the latest updated value, a client calls sync followed by read operation.

### Some final notes:

* Zookeeper is not a lock service, but it can be used by clients to implement locks. Unlike Chubby, ZooKeeper allows clients to connect to any ZooKeeper server, not just the leader.
* Zookeeper does not assume that servers can be Byzantine, but it does employ mechanisms such as checksums and sanity checks.
* Paxos may violate the FIFO client property. 
* As you add ZooKeeper Servers, the read throughput improves, but the write throughput degrades. This is because atomic broadcast needs to be done via Zab.\(a write operation requires the agreement of \(in general\) at least half the nodes in an ensemble\). Thus, Zookeeper scales well with increase in read operations, but does not with increase write operations. To alleviate this, observer replicas are used. Observers are non-voting members of an ensemble which only hear the results of votes, not the agreement protocol that leads up to them.\[8\]

Zookeeper is used for discovery, resource allocation, leader election and high priority notifications.

\[1\] Large-scale distributed applications require different forms of coordination.\(e.g. discovery, resource allocation, leader election and configurations\). See Section 2.4 for more example primitives. 

\[2\] Ephemeral znodes cannot have children. 

\[3\] When creating a new node, a client can set a sequential flag. Nodes created with the sequential flag set have the value of a monotonically increasing counter appended to its name. 

\[4\] ZooKeeper gives guarantees about ordering. Every update is part of a total ordering. All clients might not be at the exact same point in time, but they will all see every update in the same order.

\[5\] All requests from a given client are executed in the order that they were sent by the client. 

\[6\] Follower can receive requests, but all write requests are forwarded to the leader. 

\[7\] [https://distributedalgorithm.wordpress.com/2015/06/20/architecture-of-zab-zookeeper-atomic-broadcast-protocol/\#comments](https://distributedalgorithm.wordpress.com/2015/06/20/architecture-of-zab-zookeeper-atomic-broadcast-protocol/#comments) 

\[8\] [https://zookeeper.apache.org/doc/r3.4.13/zookeeperObservers.html](https://zookeeper.apache.org/doc/r3.4.13/zookeeperObservers.html)

### Reference: 

{% embed url="https://www.elastic.co/blog/found-zookeeper-king-of-coordination" %}



