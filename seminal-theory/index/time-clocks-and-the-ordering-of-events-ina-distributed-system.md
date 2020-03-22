---
description: 'https://amturing.acm.org/p558-lamport.pdf'
---

# Time, Clocks, and the Ordering of Events in a Distributed System

### Summary

Given that a global system clock or perfectly synchronized physical clocks are not available, this paper is trying to solve the problem of how to establish a global, consistent total order over all the events that occur in a distributed system without using physical clocks. 

Establishing an order in distributed is very important, for example, when the system is trying to allocate the resource to the processes in the order in which the requests are made. It is also essential to implement delivery ordering protocol and for a distributed system to achieve strong consistency.

### Definition of happens before:

A distributed system can be defined as a collection of processes. A process essentially consists of a queue of events\(which can be anything — like an instruction, or a program, or anything meaningful\). In this system, when processes communicate with each other, sending of a message is defined as an event.

We say that event A happens before event B\(A-&gt;B\) if:

1. A and B occur on the same process with A happens first
2. If A is a message sent and B is the corresponding receive
3. Transitively closed - \(A-&gt;E and E-&gt;B then A-&gt;B\)

One thing to note is that happens before relationship only defines a partial order\[1\] between events. So, we say A is concurrent with B\(A\|\|B\) if not A-&gt;B or B-&gt;A, which means A could not have caused B and B could not have caused A. 

### Logical clocks: <a id="5241"></a>

Lamport’s paper introduces a new function, essentially a counter, in every process that can assign a number to an event. Let’s call this function as Ci\(A\) as a counter in process i for event a. There are no physical clocks in the system. The function C\(A\) establishes the invariant that, event a must have happened before b, then C\(A\) &lt; C\(B\). Using this function, a partial ordering of events in the system can be established using the following two conditions:

1: Ci\(A\) &lt; Ci\(B\) if a happens before b in the same process i. This can be implemented using a simple counter in the given process.

2: When process i sends message at event a and process j acknowledges the message at event b, then Ci\(A\) &lt; Cj\(B\)

These clock functions can be thought of as ticks that occur regularly in a process. Between any two events, there needs to be at least one such tick. Each tick essentially increments the number assigned to the previous tick. 

### Note:

C\(A\) = 10 and C\(B\) = 17 doesn't imply A happens before B, but we do know B can't happen before A.  

### Partial Order vs Total Order:

An order is **total** if for any a and b in the set, either a≤b or b≤a. Less than or equal to over the integers is an example.

A **partial order** is weaker than a total order. It does not require that every pair a and b in a set can be compared

### Weakness:

As I mentioned in HLC paper summary, it’s easy to craft a case where the Lamport timestamps will order something that happened three months ago after something that happened yesterday. Thus, we need protocol with stronger guarantees.

### Related Post:

{% embed url="https://medium.com/@balrajasubbiah/lamport-clocks-and-vector-clocks-b713db1890d7" %}











