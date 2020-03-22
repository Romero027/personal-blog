---
description: 'https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf'
---

# Impossibility of Distributed Consensus with One Faulty Process

### TL;DR: Consensus is impossible

In an asynchronous network, in which at least one process can crash, any deterministic algorithm that solves consensus has executions that do not terminate. 

### Summary

Being one of the famous paper in the distributed system, this paper states and proves that consensus is impossible in the asynchronous network where at least one node can crash, known as FLP impossibility result.

The problem of consensus is to get a collection of computers to decide a single value as if they were one computer. Consensus protocol is going to attempt to satisfy three criteria of agreement\(Agreement, Validity, and Termination\)\[1\] in a fault-tolerant way and it has many application in the real world, such as transaction commit problem, which is described in the paper, leader election, reliable total order broadcast and state machine replication.

The paper starts the prove by describing the assumptions it makes. The network model is the asynchronous model, where there is no upper bound for a message to be transmitted and computation to be performed. The main problem with the asynchronous model is that there is no way to tell the difference between a failed computer and a slow computer. The failure model they chose is crash model, where at least one computer can crash, and there is no way to detect such failure.

The basic idea of the proof is to show that 1. There exist some initial configuration that is bivalent. Bivalent in the paper means the outcome is unpredictable. 2. There exists an admissible run that prevents the configuration being univalent forever.

\[1\] Agreement: All processes decide the same value\(no different readings\) Validity: The decided value is among the proposed values\(you have to choose among the inputs\) Termination: All processes eventually decide something.

### Strengths: 

1. The proof in the paper is very structured and convincing. The lemmas really helped me understand it. 

2. The authors added more constraints in their assumption\(assumed crash model instead of omission or Byzantine model.\). They showed that consensus is impossible even under perfectly reliable network.

### _**Comments**_: 

1.Are CAP theorem and FLP result somewhat related\(or even equivalent\)? It seems they both deal with safety and liveness properties in distributed systems. Specifically, agreement in consensus and strong consistency in CAP are both trying to let a set of nodes to agree with the shared state, and they are both safety properties. Likewise, termination in consensus and availability in CAP are both liveness properties. So, CAP theorem and FLP result force us to cope with the safety/liveness trade-off. Finally, although these two impossibility results are defined under different assumptions, how to tell the difference between a partitioned node and a faulty node?

### Related Links:

{% embed url="https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/" %}

{% embed url="https://www.the-paper-trail.org/post/2012-03-25-flp-and-cap-arent-the-same-thing/" %}



