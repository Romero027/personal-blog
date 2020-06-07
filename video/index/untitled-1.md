# Encoding, Fast and Slow: Low-Latency Video Processing Using Thousands of Tiny Threads

### Motivation

Video jobs take a lot of CPU and long time to finish. In addition, existing video encoders do not permit fine-grained parallelism. Video compression relies on temporal correlations among nearby frames. Splitting the video across independent threads prevents exploiting correlations that cross the split, harming compression efficiency. 

### Lambda

While cloud services like EC2 allow users to provision a cluster of powerful machines, doing so is costly: VMs take minutes to start and usage is billed with substantial minimums. Recently, cloud providers have begun offering _**microservice frameworks**_ \(e.g., AWS Lambda\) that allow systems builders to replace long-lived servers processing many requests with short-lived workers that are dispatched as request arrive. Such frameworks, especially AWS lambda, have several advantages: 

* Workers spawn quickly\(sub-second\)
* Billing is in sub-second increments\(100ms in AWS lambda\)
* A user can run many workers simultaneously\(up to thousand workers, but there is a limit\)
* Workers can run arbitrary executables

### ExCamera

This paper proposes ExCamera, a system that leverages the emerging microservice frameworks to provide low-latency video processing. Besides many workers, it has two important components external to the Lambda platform: a coordinator and a rendezvous center.

* **Coordinator**:The coordinator is a long-lived server \(e.g., an EC2 VM\) that launches jobs and controls their execution. It contains all of the logic associated with a given computation in the form of per-worker finite-state-machine \(FSM\) descriptions. For each worker, the coordinator maintains an open TLS connection, the worker’s current state, and its state-transition logic. When the coordinator receives a message from a worker, it applies the state-transition logic to that message, producing a new state and sending the next RPC request to the worker using the AWS Lambda API calls. 
* **Redezvous**: The rendezvous server that helps each worker communicate with other workers. Like the coordinator, the rendezvous server is long lived. It stores messages from workers and relays them to their destination. 
* **Workers**: The workers are short-lived Lambda function invocations. When a worker is invoked, it immediately establishes a connection to the coordinator, which thereafter controls the worker via a simple RPC interface.

### Fine-grained Parallel Video Encoding

\(Please read section 3 for some background on how video-compression works\)

The key insight of ExCamera is that the work of video encoding can be divided into fast and slow parts, with the “slow” work done in parallel across thousands of tiny threads, and only “fast” work done serially. 

#### Approach overview

1. \(Parallel\) Each thread runs a production video encoder \(vpxenc\) to encode six compressed frames,

   starting with a large key frame.

2. \(Parallel\) Each thread runs our own video encoder to replace the initial key frame with one that takes

   advantage of the similarities with earlier frames.

3. \(Serial\) Each thread “rebases” its chunk of the video onto the prior thread’s output, so that the chunks can be played in sequence by an unaltered VP8 decoder without requiring a key frame in between.

ExCamera modifies the existing VP8 encoder and decoder in explicit state-passing style. \(Section 4.1 for more details\)

####  

