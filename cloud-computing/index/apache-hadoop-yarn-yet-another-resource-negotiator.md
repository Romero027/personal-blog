---
description: 'https://www.cse.ust.hk/~weiwa/teaching/Fall15-COMP6611B/reading_list/YARN.pdf'
---

# Apache Hadoop YARN: Yet Another Resource Negotiator

### TL;DR:

**Apache Yarn** – “**Y**et **A**nother **R**esource **N**egotiator” is the resource management layer of **Hadoop**.

### Motivation:

The root of all problems was the fact that MapReduce had too many responsibilities. It was practically in charge of everything above HDFS layer, assigning cluster resources and managing job execution \(system\), doing data processing \(engine\) and interfacing towards clients \(API\). Consequently, there was no other choice for higher level frameworks other than to build on top of MapReduce.

### Architecture:

![https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html](../../.gitbook/assets/image%20%289%29.png)

The `ResourceManger` \[2\] is the master daemon that communicates with clients, tracks resources on the cluster and assigning tasks to `NodeManagers`.

`ResourceManger` has two main components: 1. Scheduler 2.Application Manager. The scheduler basically decides where the Application master will run and where these containers will be scheduled. The Application Manager manages running `ApplicationMasters` in the cluster, i.e., it is responsible for starting application masters and for monitoring and restarting them on different nodes in case of failures.

A `NodeManager` is a worker daemon that launchers and tracks processes spawned on worker hosts. The `NodeManager` will track its own local resources and communicates its resource configuration to the `ResourceManager` 

`Container` is an important YARN concept. We can think of `containers` as request to hold resources\(CPUs and Memory\) on the YARN cluster. 

For each running application, a special piece of code called an `ApplicationMaster[3][4]` helps coordinate tasks on the YARN cluster. It negotiates resources from the `ResourceManager` and works with the `NodeManager`

### Execution Overview:

1. The application starts and talks to the `ResourceManager` of the cluster.
2. The `ResourceManager` makes a single container request on behalf of the application and the `ApplicationMaster` runs within that container.
3. The `ApplicationMaster` requests subsequent containers from the `ResourceManager` that are allocated to run tasks for the application. The tasks do most of the status communication with the `ApplicationMaster` \[1\]
4. Once all tasks are finished, the `ApplicationMaster` exits. The last container is de-allocated from the cluster. 

\[1\] When the application is running, the RM is not in the loop at all. The AM directly communicates with the client and the containers it runs, which means if the RM were to crash, your application will just keep running.  

\[2\] In analogy, it occupies the place of JobTracker of Hadoop v1.

\[3\] more of a generic and efficient version of `TaskTracker`In contrast to fixed number of slots for map and reduce tasks, the NodeManager has a number of dynamically created ****resource containers. There is no hard code split available into Map and Reduce slots as in Hadoop v1

\[4\]  If a tasks\(container\) fails, the AM is responsible for updating its demand to compensate.

### Final notes:

1.When the `ResourceManager` is able to allocate a resource to the `ApplicationMaster`, it generates a lease that the `ApplicationMaster` pulls on a subsequent heartbeat. A security token associated with the lease guarantees its authenticity when the `ApplicationManager` presents the lease to the `NodeManager` to gain access to the container.

The `ApplicationMaster` heartbeats to the `ResourceManager` to communicate its changing resource needs, and to let the `ResourceManager` know it is still alive. In response, the `ResourceManager` can return a lease on additional containers on other nodes, or cancel the lease on some container

2. The Scheduler inside the RM has a pluggable policy plug-in, which is responsible for partitioning the cluster resources among the various queues, applications etc. Depending on the use case and business needs, administrators may select either a simple FIFO \(first in, first out\), capacity, or fair share scheduler. \([http://www.corejavaguru.com/bigdata/hadoop-tutorial/yarn-scheduler](http://www.corejavaguru.com/bigdata/hadoop-tutorial/yarn-scheduler)\)



### Related Post:

{% embed url="https://medium.com/@markobonaci/the-history-of-hadoop-68984a11704" %}

\*\*\*\*



