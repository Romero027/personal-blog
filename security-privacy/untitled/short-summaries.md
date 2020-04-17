# Short Summaries

### [Opaque: An Oblivious and Encrypted Distributed Analytics Platform](https://people.eecs.berkeley.edu/~wzheng/opaque.pdf) - Zheng et al., NSDI' 17

Recent proposed hardware enclaves\(e.g., Intel SGX and AMD Memory Encryption\) provide three great security properties: 1. **isolated execution**: an enclave process restricts access to a subset of memory such that only that particular enclave can access it. 2. **sealing**, which enables encrypting and authenticating the enclave's data such that no process other than the exact same enclave can decrypt or modify it. 3. **remote attestation**, which provides the ability to prove that the desired code is indeed running securely. 

Opaque is a distributed data analytics platform that utilizes Intel SGX hardware enclaves, providing strong security guarantees including computation integrity and **obliviousness**\*. Opaque operates in the query optimization layer, where it introduces a set of new distributed relational operators and novel query planning techniques.

\*\(Memory-level\) Access pattern leakage is an attack in which a compromised OS is able to infer information about the encrypted data by monitoring an application's page access. \(such leakage can also occur at the network level.\). Oblivious means the computation does not leak any access patterns.







