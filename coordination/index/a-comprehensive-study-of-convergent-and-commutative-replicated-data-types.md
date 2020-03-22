---
description: 'https://hal.inria.fr/inria-00555588/document'
---

# A comprehensive study of Convergent and Commutative Replicated Data Types

### TL;DR:

CRDT is a type of specially-designed data structure used to achieve strong eventual consistency\(SEC\) and monotonicity\(absence of rollbacks\) 

### Summary:

Replication is a fundamental problem of distributed systems. Existing works have focused on strong consistency, where replicas perform updates in the same total order. The downside of this approach is it requires consensus\[1\]. An alternative approach is to guarantee eventual consistency, which means you update the local state and propagate the updates. On conflicts, you could do merges/roll-backs.\[2\]. In eventually consistent systems, if there is a network partition or more than half of your replicas are crashed, you could still make progress.\[3\]\[4\] As we can see, both require consensus to achieve the desired property. \(reconciliation in EC requires consensus to ensure that all replicas arbitrate conflicts in the same way.

To completely avoid coordination, the authors propose strongly eventual consistency\(SEC\). An object is strongly eventual consistent if it is eventually consistent and convergence: correct replicas that have executed the same updates have the equivalent state.\[5\] In SEC systems, there will be no conflicts and the outcome of concurrent updates will be unique. There are two types of replicated systems, one is state-based and the other is operation-based. In the state-based approach, replicas will perform local updates and occasionally sends their local states to other replicas. The sufficient conditions for strong convergence in state-based objects are: 1.payload type forms a semi-lattice 2.updates are increasing\[6\] 3.merge computes least upper bound. Then replicas converge to LUB of last value. In the operation-based approach, replicas log and send operations using casually-ordered reliable broadcast protocols. The sufficient condition for convergence of an op-based object is that all its concurrent operations commute.

Finally, the authors said “SEC solves CAP problem. A SEC replica is always available for both reads and writes, independently of network conditions. Any communicating subset of replicas of a SEC object eventually converges, even if partitioned from the rest of the network. SEC is weaker than strong consistency but nonetheless provides the well-defined guarantee of strong eventual convergence. SEC provides an extreme form of fault tolerance, as a SEC object tolerates up to n − 1 simultaneous crashes. Remarkably, SEC does not require to solve consensus.”

\[1\] which means serialization bottleneck and it never tolerates more than half of faults. 

\[2\] Basically, you have to have a global decision about what the outcome will be, which actually still require consensus.

\[3\] In strongly consistent systems, consensus is in the critical path, whereas in eventually consistent systems, consensus is moved to the background.  


\[4\] However, eventually consistent systems can be hard to implement\(e.g. How to get your reconciliation right.\) 

\[5\] Instead of saying eventually they will reach the same state, strong convergence means as soon as replicas receive the same updates, they have reached the same state. \(So, there will be not roll-backs, every update is immediately and persistent.\) 

\[6\] Increasing updates mean your updates always go forward in your semi-lattice.

