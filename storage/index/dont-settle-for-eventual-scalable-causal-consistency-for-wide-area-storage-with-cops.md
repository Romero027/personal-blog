---
description: 'https://www.cs.cmu.edu/~dga/papers/cops-sosp2011.pdf'
---

# Don’t Settle for Eventual: Scalable Causal Consistency for Wide-Area Storage with COPS

### TL;DR:

This paper introduces a new consistency model, _causal+_, that extends the causal consistency model and lies between sequential and causal consistency models. The authors claim that causal+ is the _strongest_ consistency model achievable for ALPS systems, but they do not prove why something stronger cannot be achieved.

### Summary:

This paper is motivated by the problem of how to build ALPS systems which provide the strongest while achievable consistency\(Under CAP\)\[1\]. In other words, the authors are trying to develop a system that achieves both ALPS and causal consistency with convergent conflict handling.\[2\].

There are lots of existing systems, but none of them are satisfying. Amazon's Dynamo\[3\] only provides eventual consistency, which is too weak for the programmer to build on to provide other services. The inconsistent views of the updates can escape and poison those other services. On the other hand, strong-consistency \(linearizability\) is impossible due to CAP. Bayou is probably the closest work, but it does not scale well because they achieved causal+ via log-based replay. Log-exchange-based serialization inhibits replica scalability, as it relies on a single serialization point in each replica to establish ordering.

First, we need to understand what is causal consistency with convergent conflict handling\(see section 3 of the paper\). Causal consistency means the value returned from the get operation must be consistent with the order defined by causality\(i.e. "it must appear the operation that writes a value occurs after all operations that causally precede it\). However, causal consistency does not order concurrent operations and concurrent operations on the same key result in a conflict. \[4\] Thus, we want convergent conflict handling: all conflicting operations must be handled in the same manner at all replicas.\[5\].

A get operation for a record is local at the closest datacenter and is non-blocking. Since all data is replicated at each datacenter, we can have local get.

A put operation for a record is 1\) translated to put-after-dependencies based on the dependencies seen in this site 2\) queued for asynchronous replication to other sites/replicas 3\) returns done to the client at this point \(early reply\) 4\) asynchronous replication to other sites/data centers occur.

Each operation maintains dependencies for operations\(See figure 6\). Replication dependencies are checked at each datacenter, and when they are satisfied the value is updated there.

Finally, the authors present an extended version of COPS, which also supports get transactions. \[6\]

### One last thing: Why not just use vector clocks?

Answer from the authors: The problem with vector clocks and scalability has always been that the size of vector clocks in O\(N\), where N is the number of nodes. So if we want to scale to a datacenter with 10K nodes, each piece of metadata must have size O\(10K\). And in fact, vector clocks alone only allow you to learn Lamport's happens-before relationship -- they don't actually govern how you would enforce causal consistency in the system. You'd still need to employ either a serialization point or explicit dependency checking, as we do, in order to provide the desired consistency across the datacenter.

Distributed systems protocols \(and DB systems\) typically think about sites that include all the data they wish to establish causal ordering over. In COPS, a datacenter node only has a shard of the total data set, so you are fundamentally going to need to do some consistency enforcement or verification protocol.

In short: Vector clocks give you potential ordering between two observed operations. We use explicit dependency metadata to tell a server what other operation it depends on, because that operation likely resides only on other servers in the cluster!

\[1\]. ALPS means 1\)Availability: All operations issued to the data store are completed successfully. 2\) Low latency: the operations complete "quickly"\(nice to have an average performance of a few milliseconds and worst-case performance\(i.e., 99.9th\) of 10s or 100s of milliseconds\) 3\) partition tolerance: the data store continues to operate under network partitions. \(in the paper, the authors argue that network partition only occurs across datacenters 4\) High scalability: The data store scales out linearly. We know that under CAP theorem, we cannot achieve both ALPS and strong consistency\(i.e., linearizability\).  


\[2\] causal consistency is the strongest achievable consistency model under CAP theorem. 

\[3\]such systems also include Linkedin's Voldemort and Facebook's Cassandra. 

\[4\] concurrent operations mean that for operations a and b, a does not happen before b , and b does not happen before a.\) If they operate on the same key, replicas may diverge forever. 

\[5\]Common ways to handle conflicts are 1\) last-writer-win 2\) application specific means. 

\[6\]Recall that transaction is a group of operations that should occur together or not at all. Get transaction gives clients a consistent view of multiple keys.

### Strength: 

I think this is a very important paper as it presents a causally-correct system which provides a much stronger guarantee than eventual consistent systems. It also formally defines the crucial properties of distributed data stores\(ALPS\) as well as causal consistency.

### Weakness: 

The dependencies can become very big. Base on my understanding, the dependencies are stored by the client library based on the client's history. If the client submits many operations on many different keys, the dependency list can grow very long. I think one way to mitigate this, as we do in Dropbox, is to use Hybrid Logical Clocks\(HLCs\). As Murat Demirbas points out, The loosely synchronized real-time component of HLC would help in truncating the dependency list. The logical clock component of HLC would help in maintaining this dependency list precisely even in the uncertainty region of loosely synchronized clocks.

### Related links: 

On Dynamo vs. COPS vs. PNUTS 

{% embed url="https://da-data.blogspot.com/2011/12/i-stumbled-across-review-by-peter.html" %}

Review by Murat Demirbas: 

{% embed url="http://muratbuffalo.blogspot.com/2012/09/dont-settle-for-eventual-scalable.html" %}



Youtube Talk:

{% embed url="https://www.youtube.com/watch?v=jh9P1moDpAc" %}

High Scalability:

{% embed url="http://highscalability.com/blog/2011/11/23/paper-dont-settle-for-eventual-scalable-causal-consistency-f.html" %}





