---
description: 'https://syslab.cs.washington.edu/papers/tapir-tr14.pdf'
---

# Building Consistent Transactions with Inconsistent Replication

### TL;DR:

TAPIR is layered approach to supporting distribution transactions, showing that a Transactional Application Protocol can be built on top of an Inconsistent Replication protocol 

### Summary:

This paper describes a layered approach for distributed transactions. It introduces TAPIR\(“the Transaction Application Protocol for Inconsistent Replication”\), which is built on top of an inconsistent replication that provides no consistency.

Distributed transactions with strong consistency are very useful when we are building a key-value store. However, in the paper, the authors observe that combining strong consistent replication protocol\(e.g., Paxos\) and distributed transaction protocol\(e.g., 2-Phase commit\) wastes work. They both provide linearizable ordering, which leads to high latency and low throughput. Specifically, if a system that implements 2-Phase commit and Paxos\(I think Spanner is an example of such system?\), the transactions will first send to the leader in each partition and the leaders are responsible for ordering the transactions. Such design requires at least 2 RTTs to commit a transaction, and the leader will cause leader bottleneck. Thus, the goal of TAPIR is to eliminate the linearizable ordering from replication layer.

The paper co-designed two protocols. The first one\(IR\) works in replication layer. IR provides fault-tolerance and agreement\(a majority of replicas agrees on a result\) without operation ordering. In other words, agreement means the replicas will return the same result to distributed transaction protocol. IR uses Quorum-like technique to detect conflicts. IR is efficient because of no leader and coordination. Then, the paper introduces TAPIR, which is designed explicitly to work with IR's unordered operations. TAPIR uses OCC to detect conflicts and loosely synchronized clocks to order transactions. Interestingly, TAPIR uses the client as the coordinator in 2PL. Multi-versioning is implemented in TAPIR to deal with inconsistent replicas.

### Comments: 

I think the overall idea is great. TAPIR provides cheaper transaction with same guarantees. The experimental result shows that TAPIR\_KB has lower latency and better throughput than conventional systems\(e.g., Spanner, MongoDB\). However, what is not good is the complexity of the protocols. IR and TAPIR both involve sophisticated ideas and techniques. Also, if we use Paxos as replication protocol, any transaction protocol works. But, if we use IR, we need a particular transaction protocol\(like TAPIR\). Any conventional OCC or 2PL won't work.

Another interesting thing I found in this paper is that it exposes a tradeoff space between performance and programmability. the top-level protocol is very-hard-to-understand. but the savings are substantial. how many times does the top-level need to be re-implemented? this hearkens back to our conversation about Raft.

### Related Links:

Irene Zhang blog post:

{% embed url="http://irenezhang.net/blog/2015/04/02/tapir.html" %}

Irene Zhang's talk in SOSP' 15:

{% embed url="https://www.youtube.com/watch?v=yE3eMxYJDiE" %}

Review by the Morning Paper:

{% embed url="https://blog.acolyer.org/2015/10/21/building-consistent-transactions-with-inconsistent-replication/" %}



