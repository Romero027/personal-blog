# Other interesting papers

### [Efficient Queue Management for Cluster Scheduling](https://www.cse.ust.hk/~weiwa/teaching/Fall16-COMP6611B/reading_list/Yaq.pdf) - Rasley et al., EuroSys 16 

The key observation of this paper is that 1\) the RM is in the critical path of all scheduling decisions; 2\) whenever a task finishes, resources can remain fallow between heartbeats. The solution proposed is to create a queue for each node manager. However, simply maintaining a queue of tasks at worker nodes does not directly translate to benefits in JCT. Thus, the authors described techniques to 1\) determine the queue length 2\) decide the node to which each task will be placed for queuing. 3\) prioritize the task execution by reordering the queue.

However, I have some concerns about the queue management algorithm: 1\). based on my understanding, the node will notify the RM upon task completion, not through heartbeat messages. Given that the latency within datacenter is negligible, such design won't give us much benefits.  2\) the queue length is predefined  by the master. However, I feel that a dynamic queue length is better because it can deal with latency spikes and temporary master failure. 3\). Likewise, to incorporate latency variations, different nodes should have different queue length. In our work, sol, we solved a similar problem using dynamic and variable queue length. 

### [**Lineage Stash: Fault Tolerance Off the Critical Path**](https://dl.acm.org/authorize?N695036) **- Wang et al., SOSP 19**

The Motivation of this project is to provide a mechanism for fault-tolerance that has low runtime and recovery overheads. The basic idea is to ask each node forward the lineage of each of the task's inputs with the task invocation and asynchronously flush each stash to a global but physically decentralized stable storage system. This way, the node executing the task has all the information\(from the received lineage and information in the persistent storage\) to reconstruct the task's inputs, if necessary. The lineage stash looks very promising for providing lightweight fault-tolerance for fine-grain data processing systems.

