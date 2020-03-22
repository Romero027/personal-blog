---
description: 'https://www.cs.cmu.edu/~xia/resources/Documents/grandl_sigcomm14.pdf'
---

# Multi-Resource Packing for Cluster Schedulers

### Motivation:

Current schedulers neither pack tasks nor consider all their relevant resource demands. Thi results in fragmentation and overallocation of resources, respectively. 1\) Schedulers divide resources into slots \(corresponding to some amount of memory and cores\) and offer the slots greedily to the job that is furthest from its fair share\(e.g. Dominant Resource Fairness\) . Such scheduling results in _resource fragmentation_, the magnitude of which increases with the number of resources being allocated. 2\) Schedulers also ignore disk and network requirements of tasks. When assigning tasks to machines, they only check that tasks’ CPU and memory needs are satisfiable. Hence, they can schedule many network or disk-intensive tasks on the same machine.

The analysis of production workloads shows that tasks are significantly diverse in their requirements and demands of tasks for different resources are not correlated. 

### Tetris Scheduler:

The heuristic used by Teris assumes complete knowledge of the resource requirements of tasks and resource availabilities at machines. It considers demands of tasks along four resources: CPU, memory, disk, and network bandwidth. As a result, packing tasks to machines is analogous to multi-dimensional bin packing.

In one-dimensional space\(both balls and bins\), an effective heuristic proceeds by repeatedly matching the largest ball that fits in the current bin; when no more balls fit, a new bin is opened. Tetris extends this idea by defining **alignment** of a task relative to a machine across multiple dimensions. The **alignment** is defined as the dot product between task requirements and resource availabilities on machines. 

More specifically: 

> Our allocation operates as follows. When resources on a machine become available, the scheduler \*rst selects the set of tasks whose peak usage of each resource can be accommodated on that machine. For each task in this set, Tetris computes an alignment score to the machine. !e alignment score is a weighted dot product between the vector of machine’s available resources and the task’s peak usage of resources. !e task with the highest alignment score is scheduled and allocated its peak resource demands. !is process is repeated recursively until the machine cannot accommodate any further tasks.

The above algorithm has two nice properties: 1. Because Tetris uses the peak demand of each task, over-allocation is not possible. 2. If a particular resource is abundant on a machine, then tasks that require that resource will have higher scores compared to tasks that use the same amount of resources overall. 

To incorporate task placement, Tetris computes the alignment score with only local resources and imposes a remote penalty \(e.g. 10%\) to penalize use of remote resources. 

### Average Completion Time

Achieving packing efficiency does not necessarily improve job completion time. The challenges in improving job completion time is to balance prioritization of jobs that have less remaining work against loss in packing efficiency. Tetris extends the idea of shortest-remaining-time-first\(SRTF\) by scoring job's remaining work. In other word, it will calculate the total resource requirements of all remaining tasks of the job across all dimensions. Then, to combine with packing efficiency, Tetris will combine two metrics\(i.e. remaining resource usage and alignment score\) using weighted sum. For more detailed algorithm, please refer to the paper. 



### Side Note: Bin-Packing problem:





