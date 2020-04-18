# Short Summaries

### [Opaque: An Oblivious and Encrypted Distributed Analytics Platform](https://people.eecs.berkeley.edu/~wzheng/opaque.pdf) - Zheng et al., NSDI' 17

Hardware enclaves\(e.g., Intel SGX and AMD Memory Encryption\) provide three great security properties: 1. **isolated execution**: an enclave process restricts access to a subset of memory such that only that particular enclave can access it. 2. **sealing**, which enables encrypting and authenticating the enclave's data such that no process other than the exact same enclave can decrypt or modify it. 3. **remote attestation**, which provides the ability to prove that the desired code is indeed running securely. 

Opaque is a distributed data analytics platform that utilizes Intel SGX hardware enclaves, providing strong security guarantees including computation integrity and **obliviousness**\*. Opaque operates in the query optimization layer, where it introduces a set of new distributed relational operators and novel query planning techniques.

\*\(Memory-level\) Access pattern leakage is an attack in which a compromised OS is able to infer information about the encrypted data by monitoring an application's page access. \(such leakage can also occur at the network level.\). Oblivious means the computation does not leak any access patterns.

### [Prio: Private, Robust, and Scalable Computation of Aggregate Statistics](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-corrigan-gibbs.pdf) - Corrigan-Gibbs et al., NSDI' 17

This paper talks about Prio, a privacy-preserving system for the collection of aggregate statistics. Prio uses a small number of servers to collect the data; as long as one of the Prio servers is honest, the system leaks nearly nothing about client's private data. Prio also maintains robustness in the presence of an unbounded number of malicious client. 

Prio is built on a simple scheme, where each client splits its private value $$x_i$$ in to s shares, one per server, using a secret-sharing scheme. \(i.e., the sum of x shares = $$x_i$$\). For example, if there are 3 servers and $$x_i$$= 1, the client could generate \(10, -17, 8\) and send them to the corresponding server. As a result, as long as one server is honest, the client data remain private. To maintain robustness, Prio asks the clients to send to each server a "share" of proof of correctness, in which the servers can collectively verify the validity of the input. \(a variant of zero-knowledge proof?\)

### 





