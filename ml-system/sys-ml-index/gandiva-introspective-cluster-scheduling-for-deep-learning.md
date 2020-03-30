---
description: 'https://www.usenix.org/conference/osdi18/presentation/xiao'
---

# Gandiva: Introspective Cluster Scheduling for Deep Learning

### Background and Motivation

Existing schedulers mainly treat deep learning training\(DLT\) job as yet another big-data job that is allocated a set of GPUs at job startup and holds exclusive access to its GPUs until completion. However, it is inefficient for two major reasons: 1. **Head-of-line blocking**, as long DLT jobs can run for days and 2. **Low Efficiency.** The job placement is fixed at startup time, but some DLT jobs are sensitive to locality.

The authors point out several unique characteristics for DLT jobs.

#### Locality

The performance of a multi-GPU DLT job depends on the affinity of the allocated GPUs. Different DLT jobs exhibit different levels of sensitivity to inter-GPU and intra-GPU affinity. \(Two GPUs in the same machine might be located in different sockets or PCIe switches.\) 

![](../../.gitbook/assets/screen-shot-2020-03-30-at-6.41.55-pm.png)

#### Interference

When running in a shared execution environment\(e.g., PCIe switch\), DLT jobs might interfere with each other due to resource contention, and, again, different jobs have different degree of interferences. For example, when two language model\(LM\) jobs run together, both jobs suffer 19% slowdown. However, ResNet-50 does not suffer from GPU co-location with LM.

#### Intra-job predictability

A DLT jobs consists of numerous mini-batch iterations. Thus, the GPU memory used clearly follows a cyclic pattern, where each cycle corresponds to the processing of a single mini-batch. The maximum GPU memory used can be an order of magnitude larger than the minimum memory used. 



