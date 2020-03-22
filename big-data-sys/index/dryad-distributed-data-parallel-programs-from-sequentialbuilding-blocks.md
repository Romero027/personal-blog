---
description: >-
  https://www.microsoft.com/en-us/research/wp-content/uploads/2007/03/eurosys07.pdf
---

# Dryad: Distributed Data-Parallel Programs from Sequential Building Blocks

### TL;DR: 

Dryad is a general-purpose distributed execution engine for coarse-grain data-parallel applications. It achieve similar goals as MapReduce, but with different design. Computations in Dryad expressed as a graph

### Summary:

On a very high level, Dryad focuses more on simplicity of the programming model and reliability, efficiency and scalability of the application. It provides task scheduling, concurrency optimization, fault tolerance and data distribution. One of the unique feature provided by Dryad is the flexibility of fine control of an application's data flow graph.

A job in Dryad is a directed acyclic multi-graph\[3\] where each vertex is an executable program and edges represent data channels\[1\]. The job manager contains the application-specific code to construct the job’s communication graph along with library code to schedule the work across the available resources. To discover available resources, each computer in the cluster has a proxy daemon running, and they are registered into a central name server, they job manager queries the name server to get available computers.

Authors designed a simple graph description language that empowers the developer with explicit graph construction and refinement to fully take advantage of the rich features of the Dryad execution engine.

Another important design in Dryad is that each vertex belongs to a "stage", and each stage has a stage manager that receives a callback on every state transition of a vertex execution in that stage. The stage manager is used to achieve max locality, do dynamic graph refinements\[2\]

In Dryad, a scheduler inside job manager tracks states of each vertex. The vertices report status and errors to the Jon manager, and the progress of channels is automatically monitored. When a vertex execution fails for any reason, the vertex will be re-ran, but with different version numbers.

Note: 

-Partitioned distributed files: Input file expands to set of vertices, where each partition is one virtual vertex. 

-Why no cycles: Making scheduling easier! Vertex can run anywhere once all its inputs are ready and Directed-Acyclic means there is no deadlock. And Making fault-tolerance easier\(with deterministic code\): If A fails, run it again. If A's inputs are gone, run upstream vertices again. If A is slow, run another copy elsewhere and use output from whichever finishes first.

### Mapreduce versus Dryad: 

I think we can view Dryad as a general version of Mapreduce. First, computations in Dryad are not limited to just map and reduce but are express as DAGs. Second, Dryad allows communication between stages to happen over not just files stored in disk: it allows for files, TCP pipes and shared memory. Lastly, in Dryad, each vertex can take n inputs and produce n outputs, but, in Mapreduce, map only takes one input and generate one output.

In general, Dryad has a number of benefits: more efficient communication, the ability to chain together multiple stages, and express more complicated computation.

### Comments: 

I like the paper overall, but there are some weakness I'd like to point out. 1.I think it's not so simple to write programs in Dryad as you have to learn a new domain-specific language. 2. No cycles are allowed in Dryad computation. 3. Only one job can run at a time. 

As a final note, programmers aren't really meant to write program to interact with Dryad directly, but instead they are supposed to use things like DryadLINQ. This is also true for Mapreduce. FlumeJava has been heavily used at Google as an internal tool. Hive, Pig and Mahout are popular tools built on top of Hadoop. 

\[1\] Data channels can be shared memory, TCP pipes, or temp files. Temp files: The program just write to the disk, and someone else can read it later. Shard memory will pass pointers to items directly. 

\[2\] As the stage manager receives callback notifications that upstream vertices have completed, it rewrites the graph with the appropriate refinements. 

\[3\] There can be multiple edges between pairs of vertices

### Related Links:

{% embed url="https://www.youtube.com/watch?v=WPhE5JCP2Ak" %}



