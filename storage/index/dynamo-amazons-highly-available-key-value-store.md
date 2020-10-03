---
description: 'https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf'
---

# Dynamo: Amazon’s Highly Available Key-value Store

### TL;DR;

Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. It's a fully managed, multiregion, multimaster database with built-in security, backup and restore, and in-memory caching for internet-scale applications. DynamoDB can handle more than 10 trillion requests per day and can support peaks of more than 20 million requests per second.

### Summary:

This paper describes the design and implementation of Dynamo, which achieves high availability and low latency. To achieve a high level of availability, Dynamo sacrifices consistency under certain failure scenarios. The problem they were trying to solve is that given partial failure is common, how to design a storage system which provides an "always-on" experience to the user. Using a conventional relational database would lead to inefficiencies and limit scale and availability.

Dynamo is targeted mainly at applications that 1.require high availability 2. Operate in a secure network in which every node can be trusted 3. does not require hierarchical namespace or complex relational schema 4. are latency-sensitive. Dynamo uses many techniques to solve different classic problems when you design such a storage system. It provides two simple APIs to reads and writes: get\(key\) and put\(key, context\).

#### Partitioning: 

Dynamo implements a modified version of consistent hashing\[1\] to allow it scales incrementally. The problems with consistent hashing are 1. it can lead to non-uniform data and load distribution. 2. it is oblivious to the heterogeneity in the performance of nodes\(such as capacity\). To cope with these problems, Dynamo uses the concept of virtual nodes in which each node gets assigned to multiple virtual nodes in the ring\(the number of virtual nodes that a node is responsible can decided by its capacity\).

#### Replication: 

Dynamo replicates its data on multiple nodes. The node which the key is assigned and its N-1 clockwise successor\(N is configurable\). Unlike many other systems, Dynamo uses asynchronous replication protocol\[2\] to achieve high availability, which provides eventual consistency\[3\].

#### Data versioning: 

Because Dynamo uses an optimistic replication technique\[4\], it needs to be able to resolve conflicts. It uses vector clocks\[5\] to capture causality. \(The vector clock is passed as casual payload via writes\). One vector clock is associated with every version of every object. 

One thing to note is that, different from tradition vector clocks, where we have a entry in the vector for each servers, Dynamo's vector clock only consists of list of `(coordinator node, counter)`pairs. For example, `[(A,1), (B, 3)]` means server A serves one writes and server B serves three writes. 

**Handling Failures:** 

\(Partial\) Failures are common in Dynamo. It uses a "sloppy quorum" replication protocol\[6\] to handle temporarily network partition or node failure. For permanent failures, Dynamo implements an anti-entropy protocol for synchronization. To minimize data transfers, Dynamo uses Merkle trees.

#### Membership and Failure detection: 

Dynamo uses a gossip-based protocol to propagate membership changes and a local notion of failure detection to avoid communication.\[7\]

\[1\] [https://www.toptal.com/big-data/consistent-hashing](https://www.toptal.com/big-data/consistent-hashing) 

\[2\] Quorum-like replication in which it does not wait until all N-1 nodes acknowledge the write 

\[3\] Eventual consistency means "if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value" \(from Werner Vogels\). Note, there does not provide an upper bound of when the conflicts are resolved. 

\[4\] We need to be aware that under certain failure modes, Dynamo can potentially result in having not just two but several versions of the same data. 

\[5\] Dynamo also adds a timestamp to each node to reduce the size of the vector clock. 

\[6\] Reads and writes are perform on first N health nodes. Once the nodes recover from failure, other nodes\(which shouldn't have the data\) will send to the recovered node.

 \[7\] A may consider B failed if B does not respond to A's message within a time interval.

### Comments: 

I love this paper, and I think it's a must-read paper. The paper shows how many different techniques and algorithms we learned were used to build a distributed storage. It also shows how they carefully made trade-offs between availability, consistency, cost-effectiveness, and performance. 

### Update\(8/16/2019\):

I read a tech blog about vector clocks recently, called Why [Vector Clocks Are Hard](https://riak.com/posts/technical/why-vector-clocks-are-hard/)\(you should read it\). Since Dynamo makes servers act as actors, in which vector clocks of earlier read are passed as causal payloads, is Dynamo going to silently lose data some times?\(Like the example in this post.\)

Well, I think I was confused. This won't be a problem because client won't directly talk to each other. 

### Related Post:

{% embed url="https://haslab.wordpress.com/2011/07/08/version-vectors-are-not-vector-clocks/" %}

