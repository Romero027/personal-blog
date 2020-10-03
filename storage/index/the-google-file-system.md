---
description: >-
  https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf
---

# The Google File System

### TL;DR:

Google File System is a scalable distributed file system for large distributed data-intensive applications. It provides fault tolerance while running on inexpensive commodity hardware, and it delivers high aggregate performance to a large number of clients.

### Observations:

1.Component failures are the norm rather than exception  
2.Most Google Workloads only mutated files by appending to them. The workloads primarily consist of two kinds of reads: large streaming reads and small random reads.   
3.The file Google worked with are huge by traditional standards - multi-GB files are very common.   
4.The system must efficiently implement well-defined semantics for multiple clients that concurrently append to the same file.

### Summary

The authors describe the design and the performance of Google File system, which is targeted for large distributed data-intensive applications. 

The implementation details probably sound very familiar: a master and multiple chunk-servers, with files divided into replicated chunks of 64MB. The master maintains all file system metadata\[1\] in memory and uses heartbeat messages to keep an updated global view of the cluster. GFS will divide files into fixed-size chunks\[2\] and store three replicas\[3\]\[5\] for each chunk

\[1\] There are three major types of metadata: 1. The file namespaces. 2. The chunk namespaces 3. the mapping from files to chunks. \(first two types are also stored in the disk for fault tolerance\) 

\[2\] chunk size is configurable but 64MB by default\(Note large chunk size has several advantages. However, it also makes GFS bad for small files.\) 

\[3\] also configurable. Note that it may store the replicas across racks in case of rack failures. 

\[4\]It uses techniques such as not storing the chunk location persistently, make the client cache chunk locations and master replication. 

\[5\]GFS uses primary-backup-like approach for replication. " we replicate it on multiple remote machines and respond to a client operation only after flushing the corresponding log record to disk both locally and remotely"

### Strong points: 

1. I was surprised by the simplicity of the design. There's no complex algorithms/protocols involved. 

2. Although GFS has a centralized master\(which could be the bottleneck and single point of failure, they used many clever ideas\[4\] to 1.reduce the size of metadata the master need to store 2. reduce the interaction between client and the master 

3. Achieve fault tolerance

### Weakness: 

1.GFS has a relaxed consistency model to support its highly distributed operations, which means clients may see stale data. 

2. It is not optimized for small files and does not support operations other than append-only. 

3. Although it uses some techniques to reduce the workload of master, if the data size grows too fast, the master will eventually run out of memory or becomes the bottleneck. \( I guess this problem could be partially solved by having a master for each rack?\)

### Relation Links:

Hadoop File System: 

{% embed url="http://hadoop.apache.org/docs/r1.2.1/hdfs\_design.html" %}

GFS: Evolution on Fast-forward:

{% embed url="http://web.eecs.umich.edu/~mosharaf/Readings/GFS-ACMQueue-2012.pdf" %}



