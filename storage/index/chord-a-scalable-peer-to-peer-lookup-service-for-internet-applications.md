---
description: 'https://pdos.csail.mit.edu/papers/chord:sigcomm01/chord_sigcomm.pdf'
---

# Chord: A Scalable Peer-to-peer Lookup Service for Internet Applications

### TL;DR:

Chord is a non-hierarchical, distributed lookup protocol that maps a key onto a node. Chord modifies the standard consistent hashing algorithm to make the lookup more efficient and scalable.

### Summary:

The fundamental problem in the peer-to-peer protocol is that “how to locate the node that stores a particular data item efficiently.” 

### Requirements:

1.It needs to be churn-resistant. That is, when a node is added or removed from the system, we don’t want to rehash all the node and relocate lots of data. 

2. The hops need to query grows logarithmically as the number of nodes becomes 

3. The size of the routing table in each node should also increase logarithmically. 

4. It needs to be fault-tolerant. 

Previous works found that consistent hashing is the best partition algorithm and satisfies property 1 and 3. However, the underlying consistent hashing algorithm requires the nodes to know all the nodes in the system, which will cause the size of routing table grows linearly.

Chord satisfies all the properties described above with high probability. Each node maintains a pointer to its successor node, a pointer to its predecessor and a finger table to store information of other nodes. Specifically, the ith entry in the finger table contains the node that is at least 2^\(i-1\) farther. To lookup a key, if the node is not in the finger table, it will find the node that is the closest successor to the key. The algorithm for lookup is similar to the binary search in some sense.

Another contribution of the paper is the stabilization. In chord, nodes periodically verify its routing table with others in case of failure or addition of nodes. Finally, replication can be supported by the finger table. Instead of storing the data in only one node, we can store the data in its successor and successor's successor etc.

### Note:

One thing I like the finger table is that incorrect entries in the finger table will not affect the correctness of chord. We can always route by using successor pointers. It means that when a node joins or is removed, the system doesn't need to stop until all the finger tables are corrected. Instead, we can use the asynchronous anti-entropy protocol to fix the finger tables.



### Comments: 

1. Because Chord only uses IP addresses to determine the location of nodes, it doesn't consider the physical location of each node. So, even if only a few nodes are required to find the key, the lookup latency might still be unsatisfiable. 

2.The paper shows the performance of iterative Chord protocol but doesn't mention about the performance of recursive version. I'm curious if the recursive version will yield better performance in some situation. 

3.The paper doesn't convince me why not using virtual nodes. since virtual nodes have several advantages as discussed in Dynamo paper

### Related Links:

[Python](https://github.com/ultrabug/uhashring) and [Go](https://github.com/stathat/consistent) implementation of Consistent hashing. 



