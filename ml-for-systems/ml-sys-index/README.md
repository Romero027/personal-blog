# Index

### Why we need Machine Learning for Systems?

\(Paraphrase from Jeff Dean's keynote talk in SysML\) As we know, traditional low-level systems\(e.g. operating systems, compilers, and storage systems\) do not make extensive use of machine learning today. However, computer systems are filled with heuristics, which have to work well "in general cases.", but they generally don't adapt to actual patterns of usage and don't take into account available context.

For example, when BigTable receives a request to load data from a disk, it needs to decide whether to cache or not cache a particular block. Obviously, if the client is doing a sequential scan of the data, then he might never re-access that block, but if he is doing random access of the data, he will likely access the same block in the future. For example, jobs like MapReduce are very likely to do a sequential scan. We can not hard-code this type of information into the heuristics, but a learned system might actually take this information into account\(e.g. the job name and the user\).

In general, anywhere we're using heuristics to make a decision gives us an opportunity for using machine learning instead in an online manner.

* **Compilers**: instruction scheduling, register allocation, loop nest parallelization strategies..
* **Networking**: TCP window size decisions, back-off for retransmits, data compression...
* **Operating** **Systems**: process scheduling, buffer cache insertion/replacement, file system prefetching...
* **Job scheduling system**: which tasks/VM to co-locate on the same machine, which tasks to pre-empt...
* **ASIC design**: physical circuit layout, test case selection.

And anywhere we have a huge number of tunable command-line flags! 

Keys for success in these settings:

1. Having a numeric metric to measure and optimize
2. Having a clean interface to easily integrate learning into all these kinds of systems.



### Database

* \*\*\*\*[**The case for learned index structures**](https://arxiv.org/abs/1712.01208)\* - Kraska et al., SIGMOD '18
* \*\*\*\*[**SageDB: A Learned Database System**](http://cidrdb.org/cidr2019/papers/p117-kraska-cidr19.pdf)\* - Kraska et al., CIDR '19

### Networking

* \*\*\*\*[**Neural Adaptive Video Streaming with Pensieve**](https://people.csail.mit.edu/hongzi/content/publications/Pensieve-Sigcomm17.pdf) - Mao et al., SIGCOMM '17
* [**PCC Vivace: Online-Learning Congestion Control**](https://www.usenix.org/system/files/conference/nsdi18/nsdi18-dong.pdf) **-** Dong et al., NSDI '18
* \*\*\*\*[**Neural Adaptive Content-aware Internet Video Delivery**](https://www.usenix.org/system/files/osdi18-yeo.pdf) - Yeo et al., OSDI '18 
* [**Learning in situ: a randomized experiment in video streaming**](https://www.usenix.org/conference/nsdi20/presentation/yan) - Yan et al., NSDI '20 
* \*\*\*\*[**Neural-Enhanced Live Streaming: Improving Live Video Ingest via Online Learning**](https://dl.acm.org/doi/abs/10.1145/3387514.3405856) ****- Kim et al., SIGCOMM '20
* \*\*\*\*[**Interpreting Deep Learning-Based Networking Systems**](https://dl.acm.org/doi/10.1145/3387514.3405859) ****- Meng et al., SIGCOMM '20

### System

* \*\*\*\*[**Device Placement Optimization with Reinforcement Learning**](https://arxiv.org/abs/1706.04972) ****- Mirhoseini et al., ICML '17
* \*\*\*\*[**Learning Memory Access Patterns**](https://arxiv.org/pdf/1803.02329.pdf)\* - Hashemi et al., ICML '18
* \*\*\*\*[**Parity Models: Erasure-Coded Resilience for Prediction Serving Systems**](http://delivery.acm.org/10.1145/3360000/3359654/p30-kosaian.pdf?ip=35.3.50.157&id=3359654&acc=OPENTOC&key=93447E3B54F7D979%2E0A17827594E6F2C8%2E4D4702B0C3E38B35%2EC42B82B87617960C&__acm__=1572846710_212460fc2118b4ddbb56646253af114b) - Kosaian et al, SOSP '19
* \*\*\*\*[**Learning Scheduling Algorithms for Data Processing Clusters**](https://web.mit.edu/decima/content/sigcomm-2019.pdf) - Mao et al., SIGCOMM '19
* \*\*\*\*[**Learning Relaxed Belady for Content Distribution Network Caching**](https://www.usenix.org/conference/nsdi20/presentation/song) - Song et al., NSDI '20
* \*\*\*\*[**Learning-based Memory Allocation for C++ Server Workloads**](https://research.google/pubs/pub49008/) ****- Maas et al., ASPLOS '20
* \*\*\*\*[**LinnOS: Predictability on Unpredictable Flash Storage with a Light Neural Network**](https://www.usenix.org/system/files/osdi20-hao.pdf) - Hao et al, OSDI ' 20



\*denotes papers that I plan to read

