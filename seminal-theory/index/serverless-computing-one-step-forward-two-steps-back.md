---
description: 'http://cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf'
---

# Serverless Computing: One Step Forward, Two Steps Back

### What is Serverless Computing?

At the core, serverless computing boils down to function-as-a-service\(Faas\). 

* Developers can register functions to run in the cloud, and declare when those functions should be triggered
* Building applications on FaaS requires data management in both persistent and temporary storage, in addition to mechanisms to trigger and scale function execution. 
* The platform is not simply _elastic_, in the sense that humans or scripts can add and remove resources as needed, it is _autoscaling_: the workload automatically drives the allocation and deallocation of resources
* The user is only billed for the computing resources used during function invocation

The difference between traditional cloud computing and serverless is that you, the customer who requires the computing, doesn’t pay for underutilized resources. Instead of spinning up a server in AWS for example, you’re just spinning up some code execution time. The serverless computing service takes your functions as input, performs logic, returns your output, and then shuts down. You are only billed for the resources used during the execution of those functions.

### One Step Forward, Two Steps Back

By providing autoscaling, today’s FaaS offerings take a big step forward for cloud programming, offering a practically manageable, seemingly unlimited compute platform. Unfortunately, as we will see, today’s FaaS offerings also slide two major steps backward. _First, they painfully ignore the importance of efficient data processing. Second, they stymie the development of distributed systems_

In short, current FaaS solutions are attractive for simple workloads of independent tasks—be they embarrassingly parallel tasks embedded in Lambda functions, or jobs to be run by the proprietary __cloud services. However, use cases that involve stateful tasks have surprisingly high latency.

### Limitations of today's FaaS

* **Limited Lifetimes**: Typically, after 15 minutes, function invocations are shut down by the Lambda infrastructure
* **I/O Bottlenecks**: Functions are critically reliant on network bandwidth to communicate, but current platforms from AWS, Google, and Azure provide limited bandwidth.
* **Communication Through Slow storage:** Since functions are not directly network accessible, they must communicate via an intermediary service.
* **No Specialized Hardware**: FaaS offerings today only allow users to provision a time slice of a CPU hyperthread and some amount of RAM. Prediction serving relies on access to specialized hardware like GPUs, which are not available through AWS Lambda.

These constraints and some shortcomings in the standard library of Faas offerings, taken together, substantially limit the scope of feasible serverless applications. 

* _**Data-shipping**_ **architecture**: we move data to the code, not the other way round. However, it is  conventional wisdom that moving data is more expensive than moving code. 
* the high cost of function interactions _**stymies basic distributed computing**._ With all communication transiting through storage, there is no real way for thousands \(much less millions\) of cores in the cloud to work together efficiently using current FaaS platforms other than via largely uncoordinated \(embarrassing\) parallelism.

### Moving Forward:

The paper identifies some key challenges that remain in achieving a truly programmable environment for the cloud.

* Fluid Code and Data Placement: logical separation of code and data with the ability of the infrastructure to physically co-locate when it makes sense, including function shipping.
* Heterogeneous Hardware Support
* Long-running, addressable virtual agents
* Disorderly programming: The sequential metaphor of procedural programming will not scale to the cloud. Developers need languages that encourage code that works correctly in small, granular units— of both data and computation— that can be easily moved around across time and space.
* A common intermediate representation \(IR\), that can serve as a compilation target from many languages
* Service-level objectives & guarantees
* A ‘cloud-programming aware’ security model.



### Reference:

{% embed url="https://blog.acolyer.org/2019/01/14/serverless-computing-one-step-forward-two-steps-back/" %}



