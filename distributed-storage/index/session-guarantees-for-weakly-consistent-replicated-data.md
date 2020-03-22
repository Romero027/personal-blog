---
description: >-
  http://www.cs.utexas.edu/~lorenzo/corsi/cs380d/papers/SessionGuaranteesBayou.pdf
---

# Session Guarantees for Weakly Consistent Replicated Data

### TL;DR: 

Session Guarantees provides a view of the database that is consistent with their own actions, even if read and write from various, potentially inconsistent servers.

### Summary:

Eventually consistent systems are great in terms of availability. However, they may not be satisfactory for some applications seeking stronger guarantees. Session Guarantees\[1\] can easily be layered\[2\] on top of a weakly-consistent replicated data system. We assume the underlying systems are eventually consistent and symmetric, which means Reads and Writes may be performed at any server.

**Read Your Writes**: If Read R follows Write W in a session and R is performed at server S at time t, then W is included in DB\(S,t\). In other words, each user's reads should reflect the client's prior writes. This means that, for example, if I successfully add an apple to my shopping cart, it should be in the shopping cart after a page refresh.

**Monotonic Reads**: If Read R1 occurs before R2 in a session and R1 accesses server S1 at time t1 and R2 accesses server S2 at time t2, then RelevantWrites\(S1,t1,R1\) is a subset of DB\(S2,t2\). In other words, If a process performs read R1, then a later Read R2 cannot observe a state prior to the writes which were reflected in R1. It guarantees clients to observe a database that is increasingly up-to-date over time.

**Write Follow Reads**\(respect causality\) : If Read R1 precedes Write W2 in a session and R1 is performed at server S1 at time t1, then, for any server S2, if W2 is in DB\(S2\) then any W1 in RelevantWrites\(S1,t1,R1\) is also in DB\(S2\) and WriteOrder\(W1,W2\). Basically, it means causality between reads and writes are preserved.

**Monotonic Writes**: If Read R follows Write W in a session and R is performed at server S at time t, then W is included in DB\(S,t\). It basically says that writes must follow previous writes within the session.

### Definitions: 

1\) DB\(S,t\) is the ordered sequence of Writes that have been received by server S at or before time t. 

2\) RelevantWrites\(S,t,R\) denotes the function that returns the smallest set of Writes that is complete\[3\] for Read R and DB\(S,t\). 

3\) WriteOrder\(W1,W2\) be a boolean predicate indicating whether Write W1 should be ordered before Write W2.

\[1\] A session is an abstraction for the sequence of read and write operations performed during the execution of an application. For example, it could be a session which is created for each user and must exist for the lifetime of the user's account. 

\[2\] They can be provided using vector clocks. 

\[3\] We say a Write set WS is complete for Read R and DB\(S,t\) if and only if WS is a subset of DB\(S,t\) and for any set WS2 that contains WS and is also a subset of DB\(S,t\), the result of R applied to WS2 is the same as the result of R applied to DB\(S,t\).

