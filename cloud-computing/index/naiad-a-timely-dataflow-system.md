---
description: 'http://sigops.org/s/conferences/sosp/2013/papers/p439-murray.pdf'
---

# Naiad: A Timely Dataflow System

### **TL;DR:**

Naiad is a distributed system for executing data parallel, **cyclic** dataflow programs. It offers the high throughput of **batch** processors, the low latency of **stream** processors, and the ability to perform iterative and incremental computations.

### Motivation:

> Many data processing tasks require low-latency interactive access to results, iterative sub-computations, and consistent intermediate outputs so that sub-computations can be nested and composed. \(For example, an\) application that performs iterative processing on a real-time data stream, and supports interactive queries on a fresh, consistent view of the results.

We can build this by combining streaming system, batch system and triggers, but the authors argue that "application built on a single platform are typically more efficient, succinct, and maintainable."

Thus, they wanted to develop a general-purpose distributed system that is suitable for these use cases, which supports a wide variety of high-level programming models, while achieving the same performance as specialized system.

### Streaming versus Batch Computation:

Before we delve into the details of Naiad, it's first understand the difference between streaming and batch computations. For batch computation, all input for a vertex\(or stage\) must be available before that stage become active, whereas streaming computation runs asynchronously where a vertex is always active and when the input of any source comes in, a vertex can produce outcome immediately.

In general, streaming is typically more efficient because independent vertices can run without coordination, but aggregation is difficult. Batch computation supports aggregation but it require coordination, which means it's slow in normal cases.

Naiad's timely dataflow supports asynchronous and fine-grained synchronous execution. Whenever possible, the vertices can run asynchronously without any coordination, but, when necessary, vertices can synchronize to produce consistent result and simulate batch execution. 

### Naiad and Timely Dataflow:

Timely dataflow is based on a directed graph in which stateful vertices send and receive logically timestamped messages along directed edges. The graph may contain nested cycles and the timestamps reflect this structure in order to distinguish data that arise in different input epochs and loop iterations. **The resulting model supports concurrent execution of different epochs and iterations, and explicit vertex notification after all messages with a specified timestamp have been delivered.**

The graph contains input and output vertices, where the input vertices receive sequences of messages from external producers, and the output vertices send a sequence of messages out to external consumers. Producers label each message with an integer _**epoch**_**,** and notify the input vertex when they will send no more messages within the given epoch. A producer can also _close_ an input vertex which means that it will send no more messages at all \(in any epoch\). Output messages are likewise labelled with the epoch, and output vertex signals to a consumer when it will not send any more messages in an epoch.

The structure supports logical timestamps that include the epoch number and a loop iteration counter for each encountered loop. An ordering is defined for these timestamps, which corresponds to the constraint that a ‘later’ timestamped message cannot possibly be the cause of an earlier timestamped one. That is, if t1 &lt; t2; then t1 ‘happens-before’ t2. The model supports concurrent execution of different epochs and iterations.

A vertex implement two callbacks:

```text
ONRECV(e : Edge, m : Message, t: Timestamp)
ONNOTIFY(t : Timestamp)
```

ONRECV is used to deliver a message, and ONNOTIFY informs a vertex that it has received all messages for the timestamp. In addition, a vertex may invoke two system-provided methods in the context of these callbacks. 

```text
this.SENDBY(e : Edge, m : Message, t : Timestamp)
this.NOTIFYAT(t : Timestamp).
```

A vertex may call SENDBY\(e ,  : Message, t\) to send a message along an edge to another vertex, and NOTIFYAT\(t : Timestamp\) to request notification \(via ONNOTIFY\) once all messages bearing that timestamp or earlier have been delivered. 

A timely dataflow system must guarantee that v.ONNOTIFY\(t\) is invoked only after no further invocations of v.ONRECV\(e,m, t′\), for t′ ≤ t, will occur. v.ONNOTIFY\(t\) is an indication that all v.ONRECV\(e,m, t\) invocations have been delivered to the vertex, and is an opportunity for the vertex to finish any work associated with time t.

Related Links:

API: [http://microsoftresearch.github.io/Naiad/html/T\_Microsoft\_Research\_Naiad\_Dataflow\_Vertex\_1.htm](http://microsoftresearch.github.io/Naiad/html/T_Microsoft_Research_Naiad_Dataflow_Vertex_1.htm)

Review by the morning paper:[https://blog.acolyer.org/2015/06/12/naiad-a-timely-dataflow-system/](https://blog.acolyer.org/2015/06/12/naiad-a-timely-dataflow-system/)



