---
description: 'http://shivaram.org/publications/drizzle-sosp17.pdf'
---

# Drizzle: Fast and Adaptable Stream Processing at Scale

### TL;DR:

Drizzle is a system that decouples the processing interval from the coordination interval used for fault tolerance and adaptability. The experiments on a 128 node EC2 cluster show that on the Yahoo Streaming Benchmark, Drizzle can achieve end-to-end record processing latencies of less than 100ms and can get 2–3x lower latency than Spark. Drizzle also exhibits better adaptability, and can recover from failures 4x faster than Flink while having up to 13x lower latency during recovery.

### BSP and Continuous Operator:

BSP and continuous operator are two paradigms for implementing stream processing system, so let's first understand these two and the difference between them. 

**Bulk-synchronous parallel**\(BSP\) model consists of a phase whereby all parallel nodes in the system perform some local computation, followed by a blocking barrier that enables all nodes to communicate with each other, after which the process repeats itself. Examples of such system include Mapreduce, Dryad, Spark, and FlumeJava.

Streaming systems, such as Spark Streaming, Google Dataflow with FlumeJava, adopt the aforementioned BSP model. They implement streaming by creating a micro-batch of duration T seconds. During the micro-batch, data is collected from the streaming source, processed through the entire DAG of operators and is followed by a barrier that outputs all the data to the streaming sink, e.g. Kafka. Thus, there is a barrier at the end of each micro-batch, as well as within the micro-batch if the DAG consists of multiple stages, e.g. if it has a group-by operator.

An alternate computation model that is used in systems specialized for low latency workloads is **the dataflow computation model with long running or continuous operators**. Dataflow models have been used to support distributed execution in systems like Naiad, StreamScope  and Flink. In such systems, user programs are similarly converted to a DAG of operators, and each operator is placed on a processor as a long running task. As data is processed, operators update local state and messages are directly transferred from between operators. Barriers are inserted only when required by specific operators. Thus, unlike BSP-based systems, there is no scheduling or communication overhead with a centralized driver. Unlike BSP-based systems, which require a barrier at the end of a micro-batch, continuous operator systems do not impose any such barriers.

### Summary:

The problem motivates the paper is that large-scale streaming services need to provide high throughput, low latency, and adaptability\(e.g., quickly handle failure and straggler\). There exist two main execution models: 1.continuous operators\(e.g. Flink\)\[1\]. The benefit is that such design will minimize communication between worker and master.\[2\] However, such systems suffer from bad adaptability, since they have to replay everything from the last checkpoint. 2. Micro-batch model\[3\]. It provides good adaptability since you only have to replay the operations in the failed nodes\(which can even run in parallel\), but at the cost of rather high latency during normal execution\(because it imposes a hard barrier between micro-batches\).

Drizzle is built on the top of the micro-batches system but can provide both low latency and dynamic scheduling\(which means good adaptability\) by using pre-schedule reduce tasks and group schedule micro-batches. Pre-schedule reduce tasks means that we can upstream tasks are scheduled with the metadata of the information of reducer. Thus, the reducers don't need to ask the master for these metadata\(avoiding coordination\). Group scheduling means that we can reuse scheduling decisions\[4\]\(i.e., we can schedule multiple jobs at once\)and amortize the scheduling overheads.

### Weakness: 

The ideas of this paper is excellent but also very straightforward. The authors pointed out that selecting group size is a tough decision, but they didn't provide enough details\(e.g., a formula or an algorithm\) to derive the optimal group size. Also, the group size could be changed if the environment changes.

\[1\] "In such systems, user programs are similarly converted to a DAG of operators, and each operator is placed on a processor as a long-running task." 

\[2\] Because continuous operators try to eliminate hard barriers as many as possible. 

\[3\] "The computation consists of a phase whereby all parallel nodes in the system perform some local computation, followed by a blocking barrier that enables all nodes to communicate with each other, after which the process repeats itself." 

\[4\] The authors observe that "in the stream processing jobs, the computation DAG used to process micro batches is largely static, and changes infrequently."

