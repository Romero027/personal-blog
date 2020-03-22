---
description: 'http://www.neilconway.org/docs/nsdi2010_hop.pdf'
---

# MapReduce Online

### TL;DR:

Mapreduce Online proposes a modified Mapreduce architecture that allows data to be pipelined between operators and supports online aggregation, which allows user to see "early returns" from a job as it is being computed.

### Summary

In Mapreduce\(Hadoop\), a downstream worker is blocked until the previous worker process the entire data and make it available in entirety. However, MapReduce is often used for analytics on streams of data that arrive continuously. Such design will result in extremely slow job completion time.

This paper presents the Hadoop Online Prototype\(HOP\), which is a pipelined version of MapReduce. It allows maps to operate on infinite data and reduces to export early answers. To support pipelining, HOP modified the upstream task to push data to the downstream task, as it is produced. Pipelining delivers data to downstream operators more promptly. Not only the immediate downstream worker can start before the previous worker completes its work on the entire data, several other downstream workers in the chain after the immediate downstream worker can also get started as well. To fully utilize the benefit of the combiner\[1\], HOP will wait for the buffer to grow to a threshold size, instead of sending the buffer contents to reducers directly. To simplify the fault tolerance of HOP, the reducer treats the output of a pipelined map task as tentative until the master informs the reducer. Thus, the reducer could just ignore any tentative spill\[2\] files produced by the failed map attempt.

To support interactive data analysis, HOP also provides online aggregation functionality. It simply applies the reduce function to the data that a reduce task has received so far, and report the output as well as the job progress to the user.

### Comments: 

The motivation\(getting early approximate result\) seems somehow weak, since Mapreduce is often used to do batch processing. So, the benefit of early result is kind of unclear. The idea of proactively push data is great as it saves lots of I/Os imposed by the traditional MapReduce framework. However, it seems like fault tolerance are becoming more difficult. Also, multi-job online aggregation is really hard to implement, because the relation of outputs may vary.

\[1\] The role of combiners are to pre-aggregate map output locally, similar to the reduce step in word count. 

\[2\] Spill files = small sorted runs

