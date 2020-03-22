---
description: 'https://people.csail.mit.edu/matei/papers/2013/sosp_spark_streaming.pdf'
---

# Discretized Streams: Fault-Tolerant Streaming Computation at Scale

### TL;DR:

This paper introduces Spark streaming, and it sets out very clearly the problem that Discretized Streams were designed to solve: dealing effectively with _faults_ and _stragglers_ when processing streams in large clusters. 

### Summary:

The motivation of the paper is the need for processing a large stream of data in real time. Examples include website monitoring, fraud detection, and ad monetization. \(also include IoT use cases\). To support these applications. We need to design a system that is 1.Scalable 2. second-scale latency\[1\] 3. Second-scale recovery from faults and stragglers.4. High throughput\[2\]

Existing works\(e.g., Storm, Naiad\) adopt a continuous operator model. In this model, user programs are converted to a DAG of operators, and each operator is placed on a processor as a long-running task. As a processor receive input data, it updates its local state and sends messages to other operators. \[3\] However, recovering from failures in such systems are is expensive. Replication\[4\] will require too much cost, whereas upstream backup\[5\] results in slow recovery.

The idea of Discretized Streams\(D-streams\) is to structure computations as a set of short, stateless, deterministic tasks. D-stream creates micro-batches of duration T seconds. In this micro-batch model, input data is processed through the entire DAG of operators, followed by a barrier that writes the output data into the streaming sink. \(e.g., Kafka\). Each barrier requires communicating back and forth with the driver/scheduler.\[6\]

Such design greatly simplifies fault-tolerance, as the scheduler has information about tasks. The scheduler can reschedule tasks as necessary. Just like RDDs, D-streams also track their lineage. The lost partitions can be recomputed in parallel on separate nodes. We can also similarly handle stragglers. To avoid infinite replays, the system also periodically checkpoints state RDDS.

We need to understand that D-stream targets second-scale latency\(because its design to support fast recovery from faults\), but it cannot provide a few hundred milliseconds latency or below.

\[1\] The authors argue that 0.5-2 second latency is adequate. 

\[2\] There are other goals \(e.g., powerful programming model and exactly-once guarantees\) but we will not discuss them here.  
\[3\] Unlike Bulk-synchronous parallel model\(e.g., Spark Streaming\), such systems only insert barriers when required by specific operators. Thus, continuous operator streaming systems can provide lower latency during regular executions. 

\[4\] Have two copies of each node 

\[5\] Nodes buffer sent messages and replay them to a new copy of the failed node. 

\[6\] For example, the worker informs the scheduler about the allocation of output records.

### Related Links:

{% embed url="https://www.youtube.com/watch?v=yaoTzbriIJI" %}



