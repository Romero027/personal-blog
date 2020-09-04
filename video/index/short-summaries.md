# Short Summaries

### \*\*\*\*[**Potluck: Cross-Application Approximate Deduplication for Computation-Intensive Mobile Applications**](https://www.cs.yale.edu/homes/guo-peizhen/files/potluck-asplos18.pdf) ****- Guo et al., ASPLOS' 18

This paper proposes a caching mechanism to enable cross-application approximate deduplication. The feature vector of input\(e.g, a frame\) serves as the key. Thus, a lookup attempt is made with the key and the name of the function called by matching the input key to any existing key within a given similarity threshold. To strike the trade-off between performance speedup from reusing previous results and the accuracy of the results, Potluck adaptively tunes the similarity threshold. One thing to note is that Potluck only saves the result of the model and does not save any intermediate results within a model.



DeepCache: Principled Cache for Mobile Deep Vision

This paper aims to cache CNN's final results as well as its intermediate results, exploiting the temporal locality among a video stream. In other words, consecutive video frames often have substantial similar overlapped regions.

For a CNN model, DeepCache maintains a cache, covering the model’s input as well as its internal layers. The cache stores recent video frames for the model input, and recent feature maps for the internal layers. The cache keys are equal-sized, fine-grained regions on the cached input frames. The cache values are the cached feature maps produced by the layers.

Note that, Deep cache is inspired by video compression techniques and aims to cache results for a single application.

### [Vantage: optimizing video upload for time-shifted viewing of social live streams](https://dl.acm.org/doi/10.1145/3341302.3342064) - **Ray et al., SIGCOMM' 19**

Social live video streaming \(SLVS\) applications have audiences with a wide variety of viewing delays, and thus have varying degrees of latency tolerance. However, existing approaches for handling network bandwidth variations\(in mobile uplinks\) are tailored for one particular viewing delay. 

The authors analyzed a real-world trace about the variability of bandwidth in mobile upload links and observed that: periods of low/high bandwidth are common and more bandwidth is gained during high periods than is lost during low periods. 

As a result, the authors proposed a method called **quality-enhancing retransmission**. Low-quality real-time frames transmitted during periods of low bandwidth can be retransmitted at a higher bitrate during a later period of high bandwidth while the live streaming session is ongoing, thus improving the video quality for delayed, time-shifted viewers. Choosing the optimal tradeoff between the drop in real-time quality\(because of the retransmission\) and the improvement in delayed video quality can be envisioned as a quality \(SSIM\) maximization problem across the time-shifted delays



