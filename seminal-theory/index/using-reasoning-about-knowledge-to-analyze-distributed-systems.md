---
description: 'https://www.cs.cornell.edu/home/halpern/papers/UsingRAK.pdf'
---

# Using Reasoning About Knowledge to analyze Distributed Systems

### Summary:

This survey provides a conceptual framework that allows us reason about the distributed system. The author uses knowledge theory trying to make reasoning about distributed system more straightforward and more formal. Distributed systems are hard because of uncertainty.\(e.g., partial failure, ordering, and timing\). Reasoning about the distributed system, such as proving the correctness of protocol or impossibility result, is even harder.

The author begins by observing that knowledge is the core of many results and protocols in distributed system. He argues that "a protocol describes what actions to take based on a process's local knowledge." The author then introduces the notion of the possible-worlds model. He views "possibility can be viewed as the dual of knowledge." The more an agent knows, the less his uncertainty.

The most interesting result of the survey is that "common knowledge is required for simultaneous agreement\(e.g., two generals problem\) and coordinated action\(e.g., atomic commit problem\)." The given example of common knowledge in the paper is that for traffic to flow smoothly, rules need to be common knowledge. The author then gives formal mathematical semantics to explain the possible-worlds model. The core of the survey lies in a hierarchy of knowledge in which a group of processes can obtain by some protocols.

This survey gives a new and better way to analyze distributed protocols. Specifically, a process knows nothing about the world initially. As it learns about possible runs, it has some propositions hold in all these worlds. After that process learns enough possible world\(all the possible world\), it eliminates the last possibility and it has knowledge\(the proposition is true in all possible worlds, and it is a fact\). When a process sends another process a message, it shares its knowledge and eliminates possible world for other processes.

The example of using knowledge theory to analyze distributed protocol is 2-Phase Commit protocol. When all agents replied yes to the coordinator request, the coordinator knows that everyone knows the commit and when all agents sent ack to coordinators commit request, everyone knows everyone knows the commit.

### See also:

Peter Alvaro's talk: 

{% embed url="https://www.youtube.com/watch?v=7U0qPmEpbSI" %}







