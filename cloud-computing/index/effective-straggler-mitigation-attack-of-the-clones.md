---
description: 'https://www.usenix.org/system/files/conference/nsdi13/nsdi13-final231.pdf'
---

# Effective Straggler Mitigation: Attack of the Clones

### TL;DR:

This paper presents Dolly, a system that launches multiple clones tasks in jobs, completely removing waiting from straggler mitigation. Evaluation using production workloads showed that Dolly speeds up small jobs by 34% to 46% on average using only 5% extra resources.\[1\]

### Motivation:

Stragglers are tasks that run much slower than other tasks, thus increasing the latency of the corresponding jobs. Techniques of straggler mitigation can be broadly divided into two classes: **black-listing and speculative execution**. \(See related work for details\). However, the paper shows that even after applying the state-fo-the-art techniques, the small jobs still have stragglers, on average, run 8x slower than that job's median task, slowing them by 47% on average. 

Note that this paper mainly focus on small interactive jobs consist of just a few tasks\[2\], because both existing straggler mitigation techniques\(e.g. LATE and Mantri\) effectively mitigate stragglers in large jobs.

### Summary:

The paper proposes an approach that proactively launches clones of a job, just as they are submitted, and picks the result from the earliest clone. Cloning comes with two main challenges:

* Extra clones might use a prohibitive amount of extra resources. 

First, based on analysis of production traces, the smallest 90% of jobs consume as less as 6% of the resources. In addition, to guarantee that Dolly doesn't use too much resource, the scheduler will run a **Budget Cloning Algorithm\[3\]**.

* Contention in reading intermediate data\[4\].

To deal with contention, we have two straw-man solutions:

Contention-Avoidance Cloning\(CAC\): The first option eschews contention altogether. The output of an upstream task is sent to exactly one downstream task clone. Thus, the other downstream task clones have to wait for other upstream tasks to finish. The upstream stragglers will make the corresponding downstream task lag behind. Experiments show that straggler probability in a job increases by &gt;10%

 Contention Cloning\(CC\): The second option makes all tasks in a downstream clone group read the output of the upstream clone that finishes first. However, all of the downstream task clones may slow down due to contention on disk or network bandwidth. Not surprisingly, intermediate data transfer takes ~50% longer.

### Delay Assignment:

Dolly adopts a hybrid approach, similar to delay scheduling, that adds small delay to get exclusive copy before contending for the available copy. Delay is updated automatically and periodically

### Evaluation:

Dolly improves the average completion time of small jobs by 34% and 46% compare to LATE and Mantri, using fewer than 5% extra resources. In addition, Delay assignment outperforms CAC and CC by 2x. 

\[1\] After applying LATE and Mantri

\[2\] In Facebook, 88% of jobs operate on 20GB of data and contain fewer than 50 tasks

\[3\] Cloning is avoided if the cluster utilization after spawning clones will be exceed the allotted resources. The algorithm makes sure that Dolly will operate within an allotted resource budget. See section 3.2 for more details.

\[4\] Note that reading the input data is not a problem since the input data is replicated three times \(typically\) by file system and . Thus, each clone should be able to get its own copy of data. However, to avoid overheads, Intermediate data is not replicated at all.

### Related Works:

The problem of stragglers has received considerable attention already. I'll list some existing techniques and explain why they don't work well in small jobs.

**Blacklisting** identifies machines in bad health\(e.g. erroneous\) and avoid scheduling tasks on them. However, stragglers can still occur on the non-blacklisted machines, for complex reasons like IO contentions and interference by periodic maintenance operations.

**Speculative** **execution** measures relative progress of tasks of a job and launch duplicative/speculative copies of straggler tasks. Examples of such include LATE and Mantri\). However, they are ill-suited to address stragglers in small jobs, because 1. small jobs consist of just a few tasks 2. Observing might constitutes large fraction of job’s duration. 

### Related Links:

{% embed url="https://www.usenix.org/conference/nsdi13/technical-sessions/presentation/ananthanarayanan" %}



