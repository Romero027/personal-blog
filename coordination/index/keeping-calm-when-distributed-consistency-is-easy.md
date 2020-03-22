---
description: 'https://arxiv.org/abs/1901.01930'
---

# Keeping CALM: When Distributed Consistency is Easy

### Summary:

Coordination protocols, which enable distinct processes to decide how to control basic behaviors collectively, are criticized as barriers to high performance, scale, and availability of distributed systems. For example, protocols like two-phase commit may never terminate if some components failed.

### Can we avoid coordination? If so, when? 

1\)Distributed Deadlock Detection: It can be solved in a coordination-free manner because once a cycle is detected, any additional information about the system won't change the existence of a deadlock. This problem has an important property: the output grows monotonically with the input. 2\)Distributed Garbage Collection: Obviously, coordination is required in this example, as we observe that the output does not grow monotonically with the input.

-After we understand the above examples, we might ask " What is the family of problems that can be consistently\[1\] computed in a distributed fashion without coordination, and what problems lie outside that family?" The CALM\(Consistency As Logical Monoticity\) principle says that: A program has a consistent, coordination-free distributed implementation if and only if it is monotonic\[2\]\[3\] In other words: 1.logically monotonic distributed code is eventually consistent\[4\] without any need for coordination protocols \(distributed locks, two-phase commit, Paxos, etc.\) 2.eventual consistency can be guaranteed in any program by protecting non-monotonic statements \(“points of order”\) with coordination protocols.

After we understand the CALM principle, let's see its implication. -Coordination-freeness is equivalent to availability under partition: We are all frustrated about the CAP theroem because it prevents us from building a "perfect" distributed system. However, CALM circumscribes the class of programs\(i.e., monotone programs\) for which we can satisfy all three properties. -Although CALM theorem is a positive result, it's tough and time-consuming for programmers to write a truly monotone implementation of a full-featured application. Instead, we could try to keep coordination off the critical path.\[5\]

Lastly, as system builders who understand CALM theorem, we should ask us two key questions:1. Whether the problem we are trying to solve has a monotonic specification. 2. If the answer to the first question is yes, how do we implement it?

\[1\]Here, the authors define consistency as "does the program produce the outcome we expect \(e.g., deadlocks detected, garbage collected\), despite any race conditions that might arise?" 

\[2\]A program P is monotonic if, for any input set S, T, where S is a subset of T, P\(S\), is a subset of P\(T\) 

\[3\]Non-monotonic programs, which could change in the face of new information, are order-sensitive 

\[4\]one important observation of the authors is: "our true concern is with the observable behavior: the program outcomes." 

\[5\]For example, in shopping cart example, we could limit the coordination to checkout

### Update 9/9/19:

The analogy in section 1.2 is great! Take a look.

