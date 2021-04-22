# Index

## Datacenter Networks

### Architecture

* \*\*\*\*[**A scalable, commodity, data center network architecture**](http://cseweb.ucsd.edu/~vahdat/papers/sigcomm08.pdf) - Al-Fares et al., SIGCOMM '08

### RDMA

* Anuj Kalia's [**PhD thesis**](http://reports-archive.adm.cs.cmu.edu/anon/2019/CMU-CS-19-126.pdf) provides a good overview of emerging network hardware/technology
* \*\*\*\*[**Using One-Sided RDMA Reads to Build a Fast, CPU-Efficient Key-Value Store**](https://www.usenix.org/system/files/conference/atc13/atc13-mitchell.pdf) - Mitchell et al., ATC '13
* \*\*\*\*[**FaRM: Fast Remote Memory**](https://www.usenix.org/conference/nsdi14/technical-sessions/dragojevi%C4%87) - Dragojević et al., NSDI '14
* \*\*\*\*[**Design Guidelines for High Performance RDMA Systems**](https://www.usenix.org/system/files/conference/atc16/atc16_paper-kalia.pdf) - Kalia et al., ATC '16
* \*\*\*\*[**Revisiting Network Support for RDMA**](https://people.eecs.berkeley.edu/~radhika/irn.pdf) - Mittal et al., SIGCOMM '18
* \*\*\*\*[**Datacenter RPCs can be General and Fast**](https://www.usenix.org/conference/nsdi19/presentation/kalia) - Kalia et al., NSDI '19

### Operating Systems

* \*\*\*\*[**IX: A protected dataplane operating system for high throughput and low latency**](https://blog.acolyer.org/2016/06/15/ix-a-protected-dataplane-operating-system-for-high-throughput-and-low-latency/) ****- Belay et al., OSDI '14
* \*\*\*\*[**Arrakis: the operating system is the control plane** ](https://blog.acolyer.org/2016/06/14/arrakis-the-operating-system-is-the-control-plane/)- Peter et al., OSDI '14

## Wide Area Networks

### Video Streaming

* \*\*\*\*[**Improving Fairness, Efficiency, and Stability in HTTP-based Adaptive Video Streaming with FESTIVE**](http://conferences.sigcomm.org/co-next/2012/eproceedings/conext/p97.pdf) - Jiang et al., CoNEXT '12
* \*\*\*\*[**A Buffer-Based Approach to Rate Adaptation: Evidence from a Large Video Streaming Service**](http://yuba.stanford.edu/~nickm/papers/sigcomm2014-video.pdf) - Huang et al., SIGCOMM '14
* \*\*\*\*[**Neural Adaptive Video Streaming with Pensieve**](https://people.csail.mit.edu/hongzi/content/publications/Pensieve-Sigcomm17.pdf) - Mao et al., SIGCOMM '17
* [**Salsify: Low-Latency Network Video through Tighter Integration between a Video Codec and a Transport Protocol**](https://cs.stanford.edu/~keithw/salsify-paper.pdf) - Fouladi et al., NSDI '18
  * Proposes a tightly coupled codec and transport protocol
  * Exploits its codec's ability to save and restore its internal state
  * Three options when sending the next frame\(lower quality frame/higher quality frame/skip\)
  * Never send a frame unless the network is ready
* \*\*\*\*[**Neural Adaptive Content-aware Internet Video Delivery**](https://www.usenix.org/system/files/osdi18-yeo.pdf) - Yeo et al., OSDI '18 
* \*\*\*\*[**Vantage: optimizing video upload for time-shifted viewing of social live streams**](https://dl.acm.org/doi/10.1145/3341302.3342064) ****- Ray et al., SIGCOMM '19
  * Retransmit low-quality frames during high bandwidth period to improve QoE for delayed viewers
* [**Learning in situ: a randomized experiment in video streaming**](https://www.usenix.org/conference/nsdi20/presentation/yan) - Yan et al., NSDI' 20 
* \*\*\*\*[**Neural-Enhanced Live Streaming: Improving Live Video Ingest via Online Learning**](https://dl.acm.org/doi/abs/10.1145/3387514.3405856) ****- Kim et al., SIGCOMM '20

### Misc

* \*\*\*\*[**The Design Philosophy of the DARPA Internet Protocols**](http://web.stanford.edu/class/cs244/papers/DesignPhilosophyDARPA.pdf) - Clark SIGCOMM '88
* \*\*\*\*[**The Road to SDN: An Intellectual History of Programmable Networks**](https://www.cs.princeton.edu/~jrex/papers/queue14.pdf) - Feamster et al., CCR '14



