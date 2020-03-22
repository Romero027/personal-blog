---
description: 'https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf'
---

# Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing

### Summary

The paper is motivated by the problem that traditional big data frameworks\(e.g. MapReduce\) are inefficient for applications which reuse intermediate results\(e.g. iterative algorithms\(ML, SQL\)\)\[1\]. The root cause is that current frameworks require the intermediate result to be written to external storage.\[2\]

The main contribution of this paper is that it proposes a new abstraction called Resilient distributed dataset\(RDD\), which allow applications to keep working set in memory for efficient reuse, and also retain the attractive properties of MapReduce\(such as Fault-tolerance and data locality\). RDDs are 1.immutable 2. created through parallel transformations on data in \(local\) stable storage 3.can be cached\[3\]. There are two operations over RDDs: Transformation and Action. Transformation returns new RDDs, whereas Action returns some other data types\[4\].

Another major advantages of Spark is it does not store all data to provide fault-tolerance\[5\]. Instead, Spark uses the database notion of data lineage. It logs every operation performed on the data as its lineage, which can be used to reconstruct the lost partitions. It also supports checkpointing to deal with the cases that

### Note: 

1. Spark implements lazy evaluation. When we call transformation\(e.g. map\), the operation is not performed immediately. Instead, Spark will record the operations as metadata and wait until the next Action to perform the computation. Lazy evaluation reduces the number of passes it has to take over data\) 

2. Some confusions about Spark is: What will spark do if I don't have enough memory? According to Spark official documentation: "Spark's operators spill data to disk if it does not fit in memory, allowing it to run well on any sized data". It means Spark can also provide all the capabilities that Hadoop provides.

\[1\] For example, in Hadoop, stages in MapReduce start from HDFS and end at HDFS. 

\[2\] Which “ incurs substantial overheads due to data replication, disk I/O, and serialization, which can dominate application execution times.“ 

\[3\] Because RDDs are stored in memory 

\[4\] Action returns result to the driver program or write to external storage. It also leads to the actual computation. 

\[5\] In traditional systems\(such as DSM\), "the only ways to provide fault tolerance are to replicate the data across machines or to log updates across machines."



### Update:

See [here](https://xzhu0027.gitbook.io/blog/misc/deep-dive-into-the-spark-scheduler) for a deep dive into Spark's scheduler

