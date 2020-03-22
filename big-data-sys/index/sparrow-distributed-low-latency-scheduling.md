---
description: 'https://cs.stanford.edu/~matei/papers/2013/sosp_sparrow.pdf'
---

# Sparrow: Distributed, Low Latency Scheduling

### TL;DR:

Sparrow is a new scheduler that uses a decentralized, random sampling approach to provide dramatically higher throughput than the current scheduler, while also providing scheduling delays of less than 10ms and fast scheduler failover. **Sparrow does not require any communication between schedulers.** 

### Summary:

The motivation behind this paper is the trend that modern data analytic systems are running ever shorter and higher-fanout jobs\[1\]. As a result, scheduling decisions must be made at very **high throughput**.\[2\] Thus, the existing centralized scheduler cannot meet our requirements.

First, we need to understand what are the design goals. To support a workload composed of sub-second\[3\] tasks, a scheduler must:

1.Low latency: Provide millisecond-scale scheduling delay

2.High Throughput: Support millions of task scheduling decisions

3.Fault tolerant: It needs to recover from failures quickly

4.Ensure Scheduling quality: For example we might wish to distribute the tasks uniformly

The existing centralized scheduler can provide quality placement since they maintain a complete view of which tasks are running on which worker machines, but they do not meet other requirements. Sparrow adopts the decentralized approach, in which many schedulers run in parallel, and they are stateless.\[4\]\[7\]. Sparrow models each worker as a queue, the worker will run some number of tasks at a time, and if it is assign more tasks, it will add the remaining tasks to the queue

Sparrow uses a technique called **batch sampling**, instead of naive per-task sampling\[9\]. It can improve the per-task sampling because it allows all probes for a particular job to share information. To schedule using batch sampling, a scheduler randomly selects dm worker machines. \(where d&gt;=1 and m is the number of tasks within a job\), and schedule the m tasks to the m least loaded workers. 

We might argue that queue length is a poor indicator of the wait time. Consider a scenario in which node A has one 500ms task, but node b has five 10ms tasks. In addition, sampling suffers from race conditions since we have many schedulers. They might concurrently place tasks on the lightly loaded worker. To address these problems, Sparrow uses a technique called **late binding**. Instead of replying to the probes immediately after receiving it, the worker will treat the probes as tasks and place them into the queue. Thus, the previous method becomes "the scheduler assigns the job's tasks to the first m workers to reply."

### Comment: 

This paper is extremely well written as they explain their approaches very clear. As shown in\[10\], centralized scheduler will become the performance bottleneck and its scheduling quality degraded/scheduling overhead increased as the number of machines in the cluster and concurrent jobs continued to grow.

I really like the late bind idea as it solves two problems at once. However, the authors make some strong assumptions. They assume the latency is zero, which is right within a data center, but the latency across data centers may be O\(100ms\), making Sparrow not a good fit for geo-distributed data analytics. They also assume jobs are single wave, but in real-world jobs might run as multiple waves of tasks.\[8\]

Lastly, Sparrow's sampling mechanism schedule tasks on machines with the shortest queue, but it does not consider for other factors which may affect the completion time, such as data locality.

\[1\] Higher degree of parallelism 

\[2\] For example, in the paper, the authors claim that a cluster containing ten thousand 16-core machines and running 100ms tasks may require 1 million scheduling decisions per second. 

\[3\] The authors predict that the tasks are going to be shorter and shorter. 

\[4\] Some terminology: A cluster composed of worker machines, each has a fixed number of slots, and schedules. A job consists of many tasks. If the worker is currently fully utilized, it will queue new tasks until resources become available again. 

\[5\] A naive approach is to do per-task sampling by using the power of two choices. The scheduler places each task on the least loaded of two randomly selected worker machines. Note that the scheduler must first send a probe to the two randomly selected workers. 

\[7\] They might share the information across tasks when possible, but Sparrow does not require any communication between schedulers.

\[8\] Multi-wave means the number of tasks in a job or stage is greater than the number of available slots.

\[9\] The power of two choices technique proposes a simple improvement over purely random assignment of tasks to worker machines: place each task on the least loaded of two randomly selected worker machines.

\[10\] Apollo: scalable and coordinated scheduling for cloud-scale computing

### Related Links:

{% embed url="https://www.youtube.com/watch?v=A4k0WqjUY9A" %}



