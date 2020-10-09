# Index

### Video Streaming

* [**Salsify: Low-Latency Network Video through Tighter Integration between a Video Codec and a Transport Protocol**](https://cs.stanford.edu/~keithw/salsify-paper.pdf) - Fouladi et al., NSDI’ 18
  * Proposes a tightly coupled codec and transport protocol
  * Exploits its codec's ability to save and restore its internal state
  * Three options when sending the next frame\(lower quality frame/higher quality frame/skip\)
  * Never send a frame unless the network is ready
* \*\*\*\*[**Vantage: optimizing video upload for time-shifted viewing of social live streams**](https://dl.acm.org/doi/10.1145/3341302.3342064) ****- Ray et al., SIGCOMM' 19
  * Retransmit low-quality frames during high bandwidth period to improve QoE for delayed viewers

