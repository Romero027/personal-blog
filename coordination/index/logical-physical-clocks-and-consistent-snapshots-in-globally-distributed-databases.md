---
description: https://cse.buffalo.edu/tech-reports/2014-04.pdf
---

# Logical Physical Clocks and Consistent Snapshots in Globally Distributed Databases

### TL;DR:&#x20;

Hybrid Logical Clock captures the causality relationship like logical clocks, and enables easy identification of consistent snapshots in distributed systems. Dually, HLC can be used in lieu of physical/NTP clocks since it maintains its logical clock to be always close to the NTP clock.

### Summary

I read this paper when I was interning at Dropbox. I was on the Filesystem team and they were trying to cheaply provide a general consistency primitive in the Filesystem. One way is to use Logical Clock(aka Lamport Clock), but it is too weak to provide this. They provide consistency with respect to invariants (like file IDs only existing in one place), but still provide nonsensical views of data. It’s easy to craft a case where the Lamport timestamps will order something that happened three months ago after something that happened yesterday. The timestamps don’t have any grounding in reality, so they’re “allowed” to do this.

In general, 1. Logical Clock(LC) is undesirable because it is not possible to query events in relation to physical time. Thus, we will face the problem of unbound drift. 2. Vector Clock(VC) requires too much space. (i.e. in the order of nodes in the system). 3. Physical Time(PT) suffers the problem of uncertainty intervals. It may also cause the timestamps to go backwards. 4. True Time(TT) is proposed by Google in their Spanner paper. TT relies on a well engineered tight clock synchronization available at all nodes thanks to GPS clocks and atomic clocks made available at each cluster. However, TT requires special hardware and a custom-build tight clock synchronization protocol, which is infeasible for many systems. It also delay events to satisfy the invariants.

### Hybrid Logical Clock(HLC).&#x20;

HLC maintains its logical clock to be always close to NTP\[1] clock. More importantly, it preserves the property of LC.(i.e. if e happens before f then hlc.e < hlc.f) HLC can run alongside applications using NTP. Moreover, unlike NTP, HLC does not require a server-client architecture. It also works for p2p systems.

The goal of HLC is to provide one-way causality detection similar to that provided by LC, while maintaining the clock value to be always close to the physical/NTP clock.

Given a distributed system, assign each event e a timestamp, l.e, such that 1. e hb f ⇒ l.e < l.f, 2. Space requirement for l.e is O(1) integers, 3. l.e is represented with bounded space, 4. l.e is close to pt.e, i.e., |l.e − pt.e| is bounded

Please see the paper for the naive implementation and the final algorithm. The root of the unbounded drift problem(of the naive algorithm) is due to the naive algorithm using l to maintain both the maximum of pt values seen so far and the logical clock increments from new events (local, send, receive). Thus, , the l.j in the naive algorithm is expanded to two parts: l.j and c.j. The first part l.j is introduced as a level of indirection to maintain the maximum of pt information learned so far, and c is used for capturing causality updates only when l values are equal.

**Some final notes**: HLC is useful as a timestamping mechanism in multiversion distributed databases, such as Spanner. As far as I know, HLC is implement in Dropbox's new Distributed File System and [CockroachDB](https://github.com/cockroachdb/cockroach/commit/aebb70b0d3e2f0a71e06cbedef45a4fd731f5367), an open source clone of Spanner.\


\[1] Network Time Protocol: [https://www.lifewire.com/network-time-protocol-817945](https://www.lifewire.com/network-time-protocol-817945)

### Related post:&#x20;

{% embed url="http://muratbuffalo.blogspot.com/2014/07/hybrid-logical-clocks.html" %}

