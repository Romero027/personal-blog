---
description: 'https://www.usenix.org/conference/osdi18/presentation/huang'
---

# Capturing and Enhancing In Situ System Observability for Failure Detection

### The Problem 

This paper focuses primarily on catching [**gray failures**](https://xzhu0027.gitbook.io/blog/), in which some components in the system failed but the whole system typically doesn’t crash-stop. One example of such a failure is a ZooKeeper cluster that could no longer service write requests event though the leader was still actively exchanging heartbeat messages with its followers. 

As a result, the main goal of this paper is to design a failure detector that can correctly report the status of each component. The quality of such failure detector is measured by 1\)**Completeness**, which requires that if a component fails, a detector eventually suspects it; and 2\) **Accuracy**, which requires that a component is not suspected by a detector before it fails. 

### Panorama 

> The key insight of this paper is that the place where we have the most relevant information about the health of a process is in the clients that call it.

Based on this insight, Panorama takes a collaborative approach: It gathers observations about each component from different sources in real time to detect complex production failures. 



