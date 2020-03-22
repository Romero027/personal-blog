---
description: >-
  https://www.microsoft.com/en-us/research/uploads/prod/2016/12/paxos-simple-Copy.pdf
---

# Paxos made simple

### TL;DR:

Paxos is a protocol that solves the problem that getting a group of nodes to agree or reach consensus on a single value.

### Summary:

In this paper, the author introduces Paxos, which is a fault-tolerant algorithm used to reach consensus among a collection of computers in asynchronous and non-Byzantine model.

The problem of consensus is to get a collection of computers to decide a thing as if they were one computer. The main difference between consensus and other agreement problems is that consensus protocols need to be fault tolerant, which means there is no single point of failure. In contrast, in protocols like Two-Phase commit, if the coordinator were to crash, the whole system might not able to make any progress. If there is a stable leader, consensus becomes trivial, because the leader can establish a total order over all operations itself and have other nodes follow it. However, the failure of the leader will prevent the system from making any progress. Moreover, if the leader election algorithm fails\(which is likely to happen in case of network partition\), there might exist multiple leaders and violates agreement property.

The paper defines three classes of agents: 

1.Proposers\(nodes that propose value\)   
2.Acceptors\(remember the proposed values\)   
3.Learners\(discover the chosen values\). 

Then, it describes the following failed attempts to solve consensus: 

1.Single acceptor agent\(act as a leader\). Unsatisfactory because of the single point of failure. 2.Multiple acceptor agents. -acceptors accept the first value they receive -a value is chosen if it is accepted by a majority acceptors Unsatisfactory because we can easily construct cases in which no value is accepted by a majority of accepts, violating termination.   
3. Accept more than one values Unsatisfactory because the message may be delivered out of order, violating agreement.

### Paxos Algorithm:

Prepare Phase: 

For proposer, it selects a new proposal number n and broadcast a prepare request\(includes n\) to acceptors. For acceptor, it compares n with the accepted proposal with the highest number. If n is greater, the acceptor sends back a promise to follow that proposer and the highest numbered proposal\(and the value associated with the proposal\) it has accepted, if any. 

Accept Phase: 

After the proposer receives responses from a majority of acceptors, the proposer will send an accept request to all acceptors. The accept message contains the proposal number and a value\(the value of the highest numbered proposal or any value if all acceptors respond none\) When an acceptor receives an accept request, it will accept the request if the proposal number is greater than or equal to the highest numbered proposal it has accepted. Finally, if an accept request is accepted by a majority of acceptors. The proposal is chosen\(or decided\).

Note: 1.The acceptor must remember the highest numbered proposal it has accepted if any even if it fails and recovers. The proposer can forget about its proposals as long as it never tries to issue another proposal with the same proposal number 2.The proposal number needs to be globally unique and monotonically increasing 3.”Dueling proposers problem”. 

When two proposers want to propose simultaneously, they may try to block each other by issuing proposals with a number that is greater than the previous proposal. This situation could go on forever and violates termination. Possible solutions include leader election and random backoff.

### Question: 

1. What's the leader election algorithm in section 3? 

2.The paper presents several optimizations, but it doesn't explain why and how these work will improve the performance of Paxos 

3.As I read the paper, I felt that Paxos might be too difficult or costly to implement without any other optimizations 

4.The assumption that the messages cannot be corrupted seems too strong 

5.The paper seems somewhat incomplete. 1.it doesn’t address about the dueling proposers' problem described above as well as the solution. 2. How to choose the proposal number in order to make it globally unique and monotonically increasing? 3.What about membership management?

### Related Links:

{% embed url="https://www.youtube.com/watch?v=JEpsBg0AO6o" %}



