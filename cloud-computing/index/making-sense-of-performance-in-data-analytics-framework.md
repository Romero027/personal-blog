---
description: >-
  https://www.usenix.org/system/files/conference/nsdi15/nsdi15-paper-ousterhout.pdf
---

# Making Sense of Performance in Data Analytics Framework

### TL;DR:

This paper presents blocked time analysis, a methodology for quantifying performance bottlenecks in distributed computation frameworks. The result shows that \(i\) CPU \(and not I/O\) is often the bottleneck, \(ii\) improving network performance can improve job completion time by a median of at most 2%, and \(iii\) the causes of most stragglers can be identified.

### Background:

This paper is motivated by three widely-accepted mantras about the performance of data analytics:

1.The network is a bottleneck

2.The disk is a bottleneck

3.Stragglers are a major issue with unknown causes.

### Blocked time analysis:

Understanding the performance of workloads running on Spark is challenging because of two reasons: 1. One task may be bottlenecked on different resources at different points in execution, and 2. At any given time, tasks for the same job may be bottlenecked on different resources.

The authors present _**blocked time analysis,**_ which allows us to quantify how much more quickly a job would complete if tasks never blocked on the disk or the network.\[1\]

1\) Measure time when tasks are blocked on the network or disk\[2\] \(Figure 1b\)

2\) Simulate how job completion time would change. 

Subtracting these blocked times tells us the shortest possible task runtime that would result from optimizing network or disk performance. Once we have the blocked times, we can use a simulation to determine how the shorter task completion times would affect job completion time. We need to replay execution rather than simply use the original task completion times minus blocked time, since it doesn't account for multiple waves of tasks\(Figure 2\).

In summary, Blocked time analysis allow us to answer questions like how quickly could a job have completed if a resource were infinitely fast?

### Result:

The workloads used in this paper are 1. Big data benchmark\(BDBench\) 2. TPC-DS 3. Databricks production workload

1.Network optimization can only reduce job completion time by a median of at most 2%

    This is different from previous assumptions mostly because much less data\(~1/3\) is sent over the network than is transfer to and from the disk. 

2.Optimizing disk accesses can only reduce the job completion time by a median of at 19%

   CPU \(not I/O\) is often the bottleneck

3.Optimizing stragglers can only reduce job completion time by a median of at most 10%, and in 75% of queries, we can identify the cause of more than 60% of stragglers

    Previous works have a hard time understanding the underlying tasks of stragglers, but have instead focus on mitigation of stragglers. Result of this work found that the two leading cause of Spark stragglers are Java's garbage collection and time to transfer data to and from disk. Another interesting to note is that fixing the underlying causes of stragglers can speed up other tasks too. 

### Comments:

I think the main takeaway of the paper is that we should be the importance of instrumentation systems for performance analysis tools, so we can understand how best to focus performance improvements. For example, after understanding that CPU is the bottleneck, project tungsten was announced to improve the memory and disk efficiency of Spark applications. The result is less interesting than the methodology itself as the bottleneck changes over time.

\[1\] It's hard to use blocked time analysis to understand the compute time. The operating system often performs disk I/O in the background, while the task is also using the CPU.

\[2\] We focus on blocked time, rather than measuring all time when the task is using the network or the disk, because network or disk performance improvements cannot speed up parts of the task that execute in parallel with network or disk use. 



### Related link:

{% embed url="https://www.usenix.org/node/188989" %}

{% embed url="https://www.youtube.com/watch?v=mBk4tt7AEHU&list=LL88VhbuwsZoPsv87-ZLtxJQ&index=2&t=28s" %}



