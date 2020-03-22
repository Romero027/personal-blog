---
description: 'https://www.cs.rice.edu/~angchen/papers/sigcomm-2016.pdf'
---

# The Good, the Bad, and the Differences: Better Network Diagnostics with Differential Provenance

### Summary:

Distributed systems are difficult to program and near impossible to debug. Existing work\(e.g. Netsight NSDI14'\) can tell you what happened, but it's not enough for us to identify the root cause. The paper is motivated by the recent work on data provenance and the authors introduce a novel concept which they called differential provenance \(DiffProf\).

Data provenance tracks the causal connections between network states and state changes. Vertices in the provenance graph represents event or state and the edges represent the causality. Thus, the provenance tree can give us a recursive explanation of an event/state. The insight of this paper\(as well as our CIDR19' paper\) is that we can get some ideas about what went wrong by looking at the difference between the good reference and a bad symptom.

The naive solution is to do a subtraction\(finding all vertexes that are different in two trees\). However, because distributed systems are very complicated, a small initial difference will lead to a substantially different provenance tree\(butterfly effect\). Thus, the diff tree could even be larger than the original trees.

The algorithm starts with an initial equivalence relation between the packets\(establish a mapping between different packet fields.\). Then, we will create taints for equivalent fields, propagate taints up the tree and repeat until we find a non-equivalent node. We are going to roll back the execution to that non-equivalent point and change the "faulty" node to its equivalent and roll forward the execution to align the trees. We will repeat all these steps until two provenance graphs are completely equivalent. Finally, we will output out changes\(which might be the root cause of the bug\)

### Related Links:

{% embed url="https://www.youtube.com/watch?v=MlqmUyoK2hE" %}



