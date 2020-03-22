---
description: 'https://www.vldb.org/pvldb/vol8/p1792-Akidau.pdf'
---

# The Dataflow Model:A Practical Approach to Balancing Correctness, Latency, and Cost in Massive-Scale

In this summary, I will provide a detailed summary of some terminology regarding streaming data processing, the dataflow model and some other things\(e.g. Lambda architecture\). This summary is based on the dataflow [paper](https://www.vldb.org/pvldb/vol8/p1792-Akidau.pdf) and Tyler Akidau's [blog post](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101). 

### What is Streaming?

Streaming is a type of data processing engine that is designed with infinite data sets in mind. In addition, there are several other common uses of "streaming" that are worth noting.

1. **Unbounded data**: We will refer to infinite "streaming" data sets as unbounded data, and finite "batch" data sets as bounded data.
2. **Low-latency, approximate, and/or speculative results**: These types of results are most often associated with streaming engines. Batch systems have traditionally not been designed with low-latency or speculative result in mind.\(Although they are able to produce approximate results if instructed to\)

### Lambda Architecture

Streaming systems have long been relegated to a somewhat niche market of providing low-latency, inaccurate/speculative results, often in conjunction with a more capable batch system to provide eventually correct results, i.e. the [Lambda Architecture](http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html).

The Lambda Architecture looks something like this:

![From Jay Kreps](../../.gitbook/assets/image%20%281%29.png)

The basic idea is that you run a streaming system alongside a batch system, both performing essentially the same calculation. The streaming system gives you low-latency, inaccurate results \(either because of the use of an approximation algorithm, or because the streaming system itself does not provide correctness\), and some time later a batch system rolls along and provides you with correct output. However, **maintaining a Lambda system is a hassle**. You need to build, provision, and maintain two independent versions of your pipeline, and then also somehow merge the results from the two pipelines at the end. 

_Can we somehow improve the streaming processing system to handle the full problem set in its target domain? Why do you need to glue on another system?_ 

Tyler Akidau, in his blog post, argues that well-designed streaming systems actually provide a strict superset of batch functionality. \(This idea of all-streaming-all-the-time, even in batch mode, is one of the core ideas of Flink\). To beat the batch systems, you really only need two things: 

1. **Correctness**: This gets you _parity_ with batch. At the core, correctness boils down to consistent storage. Streaming systems need a method for checkpointing persistent state over time, and it must be well-designed enough to remain consistent in light of machine failures. Strong consistency is required for exactly-once processing, which is required for correctness, which is a requirement for any system that’s going to have a chance at meeting or exceeding the capabilities of batch systems.
2. **Tools for reasoning about time**: This gets you _beyond_ batch. Good tools for reasoning about time are essential for dealing with unbounded, unordered data of varying event-time skew.

### Event time vs. Processing time 

To speak cogently about unbounded data processing requires a clear understanding of the domains of time involved.

**Processing time:** Processing time refers to the system time of the machine that is executing the respective operation. 

**Event time:** Event time is the time that each individual event occurred on its producing device.

In an ideal world, event time and processing time would always be equal, with events being processed immediately as they occur. Reality is not so kind, however, and the skew between event time and processing time is not only non-zero, but often a highly variable function of the characteristics of the underlying input sources, execution engine, and hardware. As a result, if you plot the progress of event time and processing time in any real-world system, you typically end up with something that looks a bit like the red line in Figure 1.

![From Tyler Akidau ](../../.gitbook/assets/image%20%285%29.png)

In this example, the system lags a bit at the beginning of processing time, veers closer toward the ideal in the middle, then lags again a bit toward the end. The horizontal distance between the ideal and the red line is the skew between processing time and event time. That skew is essentially the latency introduced by the processing pipeline.

In the context of unbounded data, disorder and variable skew induce a completeness problem for event time windows: _lacking a predictable mapping between processing time and event time, how can you determine when you’ve observed all the data for a given event time X?_ New data will arrive, old data may be retracted or updated, and any system we build should be able to cope with these facts on its own, with notions of completeness being a convenient optimization rather than a semantic necessity.

### Data processing patterns

At this point in time, we have enough background established that we can start looking at the core types of usage patterns common across bounded and unbounded data processing today. 

**Bounded data**

Processing bounded data is quite straightforward, and likely familiar to everyone.

**Unbounded data — Batch**

Batch engines, though not explicitly designed with unbounded data in mind, have been used to process unbounded data sets since batch systems were first conceived.

* **Fixed windows**

   The most common way to process an unbounded data set using repeated runs of a batch engine is by windowing the input data into fixed-sized windows, then processing each of those windows as a separate, bounded data source. ****In reality, however, most systems still have a completeness problem to deal with: what if some of your events are delayed en route to the logs due to a network partition? This means some sort of mitigation may be necessary \(e.g., delaying processing until you’re sure all events have been collected, or re-processing the entire batch for a given window whenever data arrive late\).

* **Sessions**

   This approach breaks down even more when you try to use a batch engine to process unbounded data into more sophisticated windowing strategies, like sessions. Sessions are typically defined as periods of activity \(e.g., for a specific user\) terminated by a gap of inactivity. When calculating sessions using a typical batch engine, you often end up with sessions that are split across batches. The number of splits can be reduced by increasing batch sizes, but at the cost of increased latency. Another option is to add additional logic to stitch up sessions from previous runs, but at the cost of further complexity. 

Either way, using a classic batch engine to calculate sessions is less than ideal.

**Unbounded data — streaming**

Streaming systems are built for unbounded data. For many real-world, distributed input sources, you not only find yourself dealing with unbounded data, but also data that are: 1. **Highly unordered with respect to event times**, meaning that you need some sort of time-based shuffle in your pipeline if you want to analyze the data in the context in which they occurred. 2. **Of varying event time skew**, meaning you can’t just assume you’ll always see most of the data for a given event time X within some constant epsilon of time Y.

There are a handful of approaches one can take when dealing with data that have these characteristics. 

* **Time-agnostic**

  Time-agnostic processing is used in cases where time is essentially irrelevant — i.e., all relevant logic is data driven. Filtering is one classic example: Imagine you’re processing Web traffic logs, and you want to filter out all traffic that didn’t originate from a specific domain. Clearly, the fact that the data source is unbounded, unordered, and of varying event time skew is irrelevant.

* **Approximation algorithms**

  The second major category of approaches is approximation algorithms, such as [approximate Top-N](https://pkghosh.wordpress.com/2014/09/10/realtime-trending-analysis-with-approximate-algorithms/), [streaming K-means](https://databricks.com/blog/2015/01/28/introducing-streaming-k-means-in-spark-1-2.html), etc. They take an unbounded source of input and provide output data that, if you squint at them, look more or less like what you were hoping to get. Although approximation algorithms are designed to have low overhead, there is a limited set of them exist, the algorithms themselves are often complicated and their approximate nature limits their utility.

* **Windowing\(by processing time or event time\)**

  Windowing is simply the notion of taking a data source \(either unbounded or bounded\), and chopping it up along temporal boundaries into finite chunks for processing. Windows may be either _**aligned**_, i.e. applied across all the data for the window of the time in question, or _**unaligned**_, i.e. applied across only specific subsets of the data\(i.e. per key\) for the given window of time. 

The following diagram shows three different windowing patterns:

![From Tyler Akidau](../../.gitbook/assets/image%20%288%29.png)

**Fixed windows**: Fixed windows slice up time into segments with a fixed-size temporal length.\(e.g. hourly windows or daily windows\). They are generally aligned. 

**Sliding windows**: Sliding window are defined by a window size and slide period, e.g. hourly windows starting every minute. The period may be less than the size, which means the windows may overlap. Sliding windows are also typically aligned. 

**Sessions**: An example of dynamic windows, sessions are composed of sequences of events terminated by a gap of inactivity greater than some timeout. Sessions are commonly used for analyzing user behavior over time, by grouping together a series of temporally-related events \(e.g., a sequence of videos viewed in one sitting\). Sessions are unaligned windows.

* **Windowing by processing time**

  When windowing by processing time, the system essentially buffers up incoming data into windows until some amount of processing time has passed. For example, in the case of five-minute fixed windows, the system would buffer up data for five minutes of processing time, after which it would treat all the data it had observed in those five minutes as a window and send them downstream for processing. Windowing by processing time is simple and makes it simple to judge window completeness. 

![From Tyler Akidau ](../../.gitbook/assets/image%20%2812%29.png)

However, there is one very big downside to processing time windowing: _if the data in question have event times associated with them, those data must arrive in event time order if the processing time windows are to reflect the reality of when those events actually happened_. Unfortunately, event-time ordered data are uncommon in many real-world, distributed input sources. Thus, If you are windowing that data by processing time, your windows are will be some arbitrary mix of old and current data in terms of event time. 

* **Windowing by event time**

  ****Event time windowing is what you use when you need to observe a data source in finite chunks that reflect the times at which those events actually happened. Sadly, most data processing systems in use today lack native support for it. 

![From Tyler Akidau](../../.gitbook/assets/image%20%2817%29.png)

Another nice thing about event time windowing over an unbounded data source is that you can create dynamically sized windows, such as sessions, without the arbitrary splits observed when generating sessions over fixed windows

![From Tyler Akidau](../../.gitbook/assets/image%20%2810%29.png)

However, windowing by event time is much more expensive because: 1. **Buffering:** Due to extended window lifetimes, more buffering of data is required. Thankfully, persistent storage is generally the cheapest of the resource types most data processing systems depend on. 2. **Completeness:** Given that we often have no good way of knowing when we’ve seen _all_ the data for a given window, how do we know when the results for the window are ready to materialize? Keep this question in mind and we will discuss it in great detail later. 

### Watermarks

Watermarks are the first half of the answer to the question: “_**When**_ **in processing time are results materialized?**” Watermarks are temporal notions of input **completeness** in the event-time domain. Worded differently, they are the way the system measures progress and completeness relative to the event times of the records being processed in a stream of events

Conceptually, you can think of the watermark as a function, _F\(P\) -&gt; E_, which takes a point in processing time and returns a point in event time. That point in event time, E, is the point up to which the system believes all inputs with event times less than E have been observed. In other words, it’s an assertion that no more data with event times less than E will ever be seen again. However, gor many distributed input sources, perfect knowledge of the input data is often impractical, in which case the best option is to provide a **heuristic watermark**. Heuristic watermarks use whatever information is available about the inputs \(partitions, ordering within partitions if any, growth rates of files, etc.\) to provide an estimate of progress that is as accurate as possible. Since it's not perfectly accurate, we need to find a way to deal with late data as it comes. We highlight two shortcomings of watermarks:

1.**Too Slow**: While watermarks provide a very useful notion of completeness, depending upon completeness for producing output is often not ideal from a latency perspective.\(Results are delayed\)

2.**Too fast**: When a heuristic watermark is incorrectly advanced earlier than it should be, it’s possible for data with event times before the watermark to arrive some time later, creating late data.

Addressing these shortcomings is where triggers come into play. 

### Triggers

Triggers are the second half of the answer to the question: “_**When**_ **in processing time are results materialized?**” Triggers declare when output for a window should happen in processing time. Each specific output for a window is referred to as a _pane_ of the window. Examples of signals used for triggering include:

* **Watermark progress \(i.e., event time progress\),** where outputs were materialized when the watermark passed the end of the window
* **Processing time progress**, which is useful for providing regular, periodic updates since processing time \(unlike event time\) always progresses more or less uniformly and without delay.
* **Element counts**, which are useful for triggering after some finite number of elements have been observed in a window.
* **Punctuations**, or other data-dependent triggers, where some record or feature of a record \(e.g., an EOF element or a flush event\) indicates that output should be generated.

In addition to simple triggers that fire based off of concrete signals, there are also composite triggers that allow for the creation of more sophisticated triggering logic. Thus, in the **too slow** case,  triggering periodically when processing time advances \(e.g., once per minute\) is a good idea. In the **too fast** case, triggering after observing an element count of 1 will give us quick updates to our results \(i.e., immediately any time we see late data\), but is not likely to overwhelm the system given the expected infrequency of late data.

### **Accumulation**

When triggers are used to produce multiple panes for a single window over time\(because the existence of late data, we find ourselves confronted with the last question: “_**How**_ **do refinements of results relate?**” 

* **Discarding:** Every time a pane is materialized, any stored state is discarded.
* **Accumulating:** Every time a pane is materialized, any stored state is retained, and future inputs are accumulated into the existing state.
* **Accumulating & retracting:** Like accumulating mode, but when producing a new pane, also produces independent retractions for the previous pane\(s\).

### Case Study - Apache Flink

That’s it for the concepts! In the remaining sections, I will provide some case studies related to the topics above and the first example we will be looking at is Apache Flink. 

Like I mentioned, Flink folks built a system that’s all-streaming-all-the-time under the covers, even in “batch” mode. Essentially, Flink implements many techniques from the Dataflow Model. Here, I will focus on late elements in Flink. 

The mechanism in Flink to measure progress in event time is **watermarks**. Watermarks flow as part of the data stream and carry a timestamp _t_. A _Watermark\(t\)_ declares that event time has reached time _t_ in that stream, meaning that there should be no more elements from the stream with a timestamp _t’ &lt;= t_ \(i.e. events with timestamps older or equal to the watermark\).

Watermarks are crucial for _out-of-order_ streams, as illustrated below, where the events are not ordered by their timestamps. In general a watermark is a declaration that by that point in the stream, all events up to a certain timestamp should have arrived. Once a watermark reaches an operator, the operator can advance its internal _event time clock_ to the value of the watermark.

![](../../.gitbook/assets/image%20%2813%29.png)

To reiterate, it is possible that certain elements will violate the watermark condition, meaning that even after the _Watermark\(t\)_ has occurred, more elements with timestamp _t’ &lt;= t_ will occur. Late elements are elements that arrive after the system’s event time clock \(as signaled by the watermarks\) has already passed the time of the late element’s timestamp. 

By default, late elements are dropped when the watermark is past the end of the window. However, Flink allows to specify a maximum _allowed lateness_ for window operators. Allowed lateness specifies by how much time elements can be late before they are dropped, and its default value is 0. Elements that arrive after the watermark has passed the end of the window but before it passes the end of the window plus the allowed lateness, are still added to the window. Depending on the trigger used, a late but not dropped element may cause the window to fire again. 

In order to make this work, Flink keeps the state of windows until their allowed lateness expires. Once this happens, Flink removes the window and deletes its state. \(Worded differently, a window is **created** as soon as the first element that should belong to this window arrives, and the window is **completely removed** when the time \(event or processing time\) passes its end timestamp plus the user-specified `allowed lateness`\). 

### Comments:

I liked the paper overall. The graphs in the paper\(especially figure 2\) are straightforward and informative. However, I not fully convinced about the argument that streaming is a strict superset of batch computation. When all the data has been collected before it needs to be processed and latency is not the top priority, batch computation systems provide better throughput and fault-tolerance/adaptivity\(Please refer to Drizzle paper about the detailed comparison\). 

One thing I don't like about the paper or the blog post is that there is no evaluation/performance test at all, but I guess they might be adequately discussed in FlumeJava or MillWheel since I haven't read them yet. 

### References:

{% embed url="https://ci.apache.org/projects/flink/flink-docs-stable/dev/event\_time.html" %}

{% embed url="https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101" %}

{% embed url="https://www.youtube.com/watch?v=3UfZN59Nsk8" %}

