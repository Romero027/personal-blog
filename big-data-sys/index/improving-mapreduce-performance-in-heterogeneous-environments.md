---
description: >-
  http://courses.cs.vt.edu/cs5204/fall12-kafura/Papers/MapReduce/Map-Reduce-Hadoop.pdf
---

# Improving MapReduce Performance in Heterogeneous Environments

### TL;DR:

This paper presents Longest Approximate Time to End\(LATE\), which is a scheduling algorithm highly robust to heterogeneous network. LATE is based on three principles: prioritizing tasks to speculate, selecting fast nodes to run on, and capping speculative tasks to prevent thrashing.

### Motivation:

Stragglers are tasks that run much slower than other tasks, thus increasing the latency of the corresponding jobs. 

#### Speculative Execution in Hadoop:

When the Hadoop scheduler tries to schedule the next task to run, any failed tasks are give highest priority. Second, non-running tasks are considered. Finally, Hadoop looks for a task to execute speculatively. 

To select speculative tasks, Hadoop monitors task progress using a _progress score_ between 0 and 1. For a map, the `progress score` is the fraction of input data read. For a reduce task, the execution is divided into three phases\(copy, sort and reduce\), each of which accounts for 1/3 of the score. In each phase, the score is the fraction of data processed.

Hadoop looks at the average progress score of each category of tasks \(maps and reduces\) to define a threshold for speculative execution: When a task’s progress score is less than the average for its category minus 0.2, and the task has run for at least one minute, it is marked as a straggler.\[1\] The threshold works reasonably well in homogenous environments

#### Assumptions in Hadoop Scheduler and why they breaks:

> 1. Nodes can perform work at roughly the same rate.
> 2. Tasks progress at a constant rate throughout time.
> 3. There is no cost to launching a speculative task on a
>
>    node that would otherwise have an idle slot.
>
> 4. A task’s progress score is representative of fraction
>
>    of its total work that it has done. Specifically, in a
>
>    reduce task, the copy, sort and reduce phases each
>
>    take about 1/3 of the total time.
>
> 5. Tasks tend to finish in waves\[2\], so a task with a low
>
>    progress score is likely a straggler.
>
> 6. Tasks in the same category \(map or reduce\) require
>
>    roughly the same amount of work

The first two assumptions naturally breaks in heterogeneous environment\[3\]. Assumption 3 breaks down when resources are shared. Also, too many speculative tasks may be launched, taking away resources from useful tasks. Assumption 4 is untrue because the copy phase of reduce tasks is the slowest, but the copy phase counts for only 1/3 of the progress score. Lastly, the 20% progress difference threshold used by Hadoop’s scheduler means that tasks with more than 80% progress can never be speculatively executed.

\[1\] Note that all tasks beyond the threshold are considered "equally slow". 

\[2\]  If the number of tasks is greater than the number of available slots in the cluster, the task assignment proceeds in multiple rounds; each round is called an execution wave.

\[3\] The authors argue lost of factors which cause heterogeneity, but the one I found most interesting is contention. Although virtualization isolates CPU and memory performance, but VMs compete for disk and network bandwidth. 

### LATE:

The key insight of LATE is it always speculatively execute the task that we think will finish _farthest into the future._

#### Definitions:

* The estimate task completion time is `(1 - ProgressScore)/ProgressRate` where the _progress rate_ of each task is defined as `ProgressScore/T` and T is the amount of time the task has been running for. LATE assumes that tasks make progress at a roughly constant time.\[4\]
* To make sure we backup tasks on fast nodes, LATE will not launch speculative tasks on nodes that are below `SlowNodeThreshold` \[5\] of total work performed. It is defined as the sum of progress score of all tasks on the node. 
* To avoid using too much resources, LATE has a cap on the number of speculative tasks that can be running at once, which is denoted as `SpeculativeCap` and a `SlowTaskThreshold` that a task’s progress rate is compared with to determine whether it is “slow enough” to be speculated upon.

### The algorithm:

> If a node asks for a new task and there are fewer than `SpeculativeCap`speculative tasks running: 1. Ignore the request if the node’s total progress is below `SlowNodeThreshold`. 2. Rank currently running tasks that are not currently being speculated by estimated time left. 3. Launch a copy of the highest-ranked task with progress rate below `SlowTaskThreshold`

Obviously, LATE is robust to node heterogeneity. More importantly, LATE considers node heterogeneity when deciding where to run speculative tasks. Lastly, by focusing on estimated time left rather than progress rate, LATE speculatively executes only tasks that will improve job response time, rather than any slow tasks. One thing to note is that LATE does not take data locality into account.

\[4\] It is a valid assumption since a map task’s progress is based on the number of records it has processed and a reduce tasks are typically slowest in their copy phase, but speed up over time. 

\[5\] `SpeculativeCap` to 10% of available task slots and `SlowNodeThreshold` and `SlowTaskThreshold` to the 25th percentile of node progress and task progress rates respectively is a good choice in practice. 

### Comments:

If I understand correctly, the LATE algorithm does not account for input skew\(Partitioning data over a low entropy key space often leads to a skew in the input sizes of tasks.\) For example, if task A has been run for 30s and read 1/3 of its input data, whereas task B has been run for 60s and also read 1/3 of its input data, task A and B will have estimated completion time 60s and 120s respectively. However, task B's runtime might be well explained by amount of data they process or move on the network. Thus, it should not be duplicated. 

In general, Duplicating these tasks would not make them run faster and will waste resources. Further, a duplicate may finish faster than the original task but has the opportunity cost of consuming resources that other pending work could have used.  



