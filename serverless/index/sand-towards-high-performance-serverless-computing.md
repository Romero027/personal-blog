# SAND: Towards High-Performance Serverless Computing

SAND is a serverless computing system that provides lower latency, better resource efficiency, and more elasticity than existing serverless platforms. The key techniques in SAND are 1\) application-level sandboxing, and 2\) a hierarchical message bus.

### Problem with existing serverless systems

First, most existing serverless platforms execute each application within a separate container instance. This approach can either suffer from high startup latency\(i.e., cold start\) or resource inefficiency\(i.e., running idle when keeping a launched container warm\).

Second, existing serverless platforms do not consider interactions among functions. Events that trigger function executions can be categorized as external \(e.g., a user request calling a function sequence\) and internal \(e.g., a function initiating other functions during the workflow execution\). Existing serverless platforms normally treat these events the same, which means all events have to traverse the full end-to-end function call path, incurring undesired latencies.

