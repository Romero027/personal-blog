---
description: 'http://elmeleegy.com/khaled/papers/delay_scheduling.pdf'
---

# Delay Scheduling: A Simple Technique for Achieving Locality and Fairness in Cluster Scheduling

### TL;DR:

Delay Scheduling is an algorithm to get a good tradeoff point between the conflicts of fairness and data locality, which practically improve response time for small jobs by 5x in a multi-user workload and double throughput in an IO-heavy workload.

### Summary:

This paper is motivated by the problem of sharing a cluster between users while preserving the efficiency of the system. In other words, how to make tradeoffs between fairness in scheduling and data locality. Hadoop's default scheduler runs jobs in FIFO order, which is bad when many clients are using Hadoop.

The authors highlighted two main goals of their design. 1. "Divide resources using max-min fair sharing\[1\] to achieve statistical multiplexing" 2. "Place computations near their input data, to maximize system throughput"

The paper presents an algorithm based on waiting to achieve both high fairness and high data locality, which they called it "delay scheduling". The algorithm will let a job waits for a limited amount of time for a scheduling opportunity on a node that has data for it\[2\].

The authors first present the naive version of the scheduling algorithm and explain some problems\[3\] of it regarding data locality. Then, they explain the idea of delay scheduling: when a node has a free slot and the head-of-line job cannot launch a local task, we skip it and try to schedule subsequent jobs. To avoid starvation, the algorithm set a skip count D which we allow jobs to be skipped up to D times.

### Strength: 

1. The overall algorithm is simple and the straw-man solution really helped me understand the delay scheduling algorithm. 

2. The authors consider both rack locality and node locality, based on the observation that running jobs on the same rack is faster than running off-rack.

### Weakness: 

1. Delay scheduling may not work well if the jobs are long

\[1\] [https://www.ece.rutgers.edu/~marsic/Teaching/CCN/minmax-fairsh.html](https://www.ece.rutgers.edu/~marsic/Teaching/CCN/minmax-fairsh.html) 

\[2\] The authors show that "A very small amount of waiting is enough to bring locality close to 100%". Based on the workload traces from Facebook, most tasks are short and most jobs are small. \(See the paper for definitions of small and short\) 

\[3\] Head-of-line scheduling and sticky slots

### Related Links:

{% embed url="http://hexianghu.com/big-data/2015/03/01/delay-scheduling/" %}

