---
description: 'https://scholar.google.com/scholar?cluster=17642052422667212790'
---

# CAP Twelve Years Later: How the “Rules” Have Changed

### TL;DR: 

  System designers should not blindly sacrifice consistency or availability when partitions exist. The author proposes a strategy allows the database to continue to provide availability during a partition, and enforce consistency once the partition is resolved.

### Recapping CAP, ACID and BASE:

CAP: 

-C\(Consistency\): meaning a single up-to-date copy of the data exists.\[1\]

 -A\(Availability\): every request receives a \(non-error\) response 

-P\(Partition Tolerance\): the system operates in the face of network partitions

  In essence, the CAP theorem prohibits the design of a database with “perfect availability and consistency in the presence of partitions”. The relationship between CAP and ACID is complex and often misunderstood, in part because the C and A in ACID represent different concepts than the same letters in CAP and in part because choosing availability affects only some of the ACID guarantees.

ACID: 

-A\(Atomicity\): All systems benefit from atomic operations.\(Both "CA" and "CP"\) 

-C\(Consistency\): C in ACID means that a transaction preserves all the database rules, such as unique keys. Although different than C in CAP, but ACID consistency also cannot be maintained across partitions. 

-I\(Isolation\): Isolation is at the core of the CAP theorem: if the system requires ACID isolation, it can operate on at most one side during a partition. Serializability requires communication in general and thus fails across partitions. 

-D\(Durability\): As with atomicity, there is no reason to forfeit durability

  ACID and BASE represent two design philosophies at opposite ends of the consistency-availability spectrum.

BASE: 

-BA\(Basically Available\): the system guarantee availability 

-S\(Soft State\): the state of the system may change over time 

-E\(Eventual Consistency\): the system will be consistent over time.

### Why "2 of 3" is misleading?

  First, because partitions are rare, there is little reason to forfeit C or A when the system is not partitioned. Second, the choice between C and A can occur many times within the same system at very fine granularity; not only can subsystems make different choices, but the choice can change according to the operation or even the specific data or user involved. Finally, all three properties are more continuous than binary. Availability is obviously continuous from 0 to 100 percent, but there are also many levels of consistency, and even partitions have nuances, including disagreement within the system about whether a partition exists

### Managing Partitions:

  The challenging case for designers is to mitigate a partition’s effects on consistency and availability. The key idea is to manage partitions very explicitly, including not only detection, but also a specific recovery process and a plan for all of the invariants that might be violated during a partition. This management approach has three steps:

-Detect the start of a partition, 

-Enter an explicit partition mode that may limit some operations\[2\], and 

-Initiate partition recovery when communication is restored.

Once the system enters partition mode, two strategies are possible. The first is to limit some operations, thereby reducing availability. The second is to record extra information about the operations that will be helpful during partition recovery.

### Partition Recovery:

At some point, communication resumes and the partition ends. At this point, the system knows the state and history of both sides because it kept a careful log during partition mode. It is generally easier to fix the current state by starting from the state at the time of the partition and rolling forward both sets of operations in some manner, maintaining consistent state along the way. Bayou did this explicitly by rolling back the database to a correct time and replaying the full set of operations in a well-defined, deterministic order so that all nodes reached the same state.\[3\]

\[1\] More formally, Consistency means linearizability, writes should appear to be instantaneous. Imprecisely, once a write completes, all later writes should return the value of that write or the value of a later write. 

\[2\] Designers can choose to constrain the use of certain operations during partitioning so that the system can automatically merge state during recovery\(e.g. CRDTs\). 

\[3\] Note that most system cannot always merge conflicts and requires manual resolution, while some systems can always merge conflicts by choosing certain operations\(e.g. Google Docs\). Using commutative operations is the closest approach to a general framework for automatic state convergence

### Other interesting post about "CAP": 

* \*\*\*\*[Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
* [You Can’t Sacrifice Partition Tolerance](https://codahale.com/you-cant-sacrifice-partition-tolerance/)



