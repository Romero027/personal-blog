---
description: 'https://www.usenix.org/system/files/nsdi19-gu.pdf'
---

# Tiresias: A GPU Cluster Managerfor Distributed Deep Learning

### Background and Motivation

The authors argue three primary challenges faced by distributed deep learning\(DDL\) scheduler in production.

#### Unpredictable job duration

Some of the existing schedulers try to predict the DL job training times by assuming DL jobs have smooth loss curves. However, because of the trial-and-error characteristic of DL jobs, their loss curves are not as smooth as the curves of the best model ultimately picked at the end of exploration. Thus, the scheduler should not rely on the loss curve for predicting eventual job completion time. 

#### Over-aggressive job consolidation

Because DL jobs are sensitive to GPU locality, many existing solutions assign all components of the job to the same or the minimum number of servers. As a result, jobs often wait when they cannot be consolidated, even if there are enough spare resources elsewhere in the cluster. 

#### Time overhead of preemption

The common way to preempt Unlike preemption in CPU, GPU preemption usually takes tens of milliseconds. 

### Tiresias

The main objectives of Tiresias are 1\) minimizing the average job completion time\(JCT\), 2\) achieving high GPU utilization and 3\) avoiding starvation. 

![](../../.gitbook/assets/screen-shot-2020-03-30-at-9.48.20-pm.png)

To address the aforementioned challenges, Tiresias uses an aged based scheduler called **Two-dimensional Attained Service-Based Scheduler**\(2DAS\). 2DAS assigns each job a priority based on its **attained service**. The attained service of a job is calculated based on the number of GPUs it uses and the amount of time it has been running so far. 

When no jobs duration information is provided, the priority function applies the Least-Attained-Service\(LAS\) algorithm where a job's priority is inverse to its attained service. If the distribution of job duration is provided, then a job's priority equals its Gittins index value. 

Using continuous priorities can lead to a sequence of preemptions\(preemption is both time-consuming and expensive in GPUs\) and subsequent resumptions for all jobs. Tiresias address this challenge by using the classic **Multi-Level Feedback Queue algorithm**. 

![](../../.gitbook/assets/screen-shot-2020-03-30-at-10.04.42-pm.png)

Another insight of Tiresias is that **the skew of the model structure** can be a good predictor of whether a job is sensitive to consolidation, because the message size distribution in DLL depends on the tensor size distribution of the model. Based on this insight, Tiresias profiler identifies the amount of skew in tensor distributions across parameter servers and if it is larger than a predefined threshold, Tiresias attempts to consolidate the job in as few machines as possible. 

### 

