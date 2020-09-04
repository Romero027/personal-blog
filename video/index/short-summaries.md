# Short Summaries

### [Vantage: optimizing video upload for time-shifted viewing of social live streams](https://dl.acm.org/doi/10.1145/3341302.3342064) - Ray et al., SIGCOMM' 19

Social live video streaming \(SLVS\) applications have audiences with a wide variety of viewing delays, and thus have varying degrees of latency tolerance. However, existing approaches for handling network bandwidth variations\(in mobile uplinks\) are tailored for one particular viewing delay. 

The authors analyzed a real-world trace about the variability of bandwidth in mobile upload links and observed that: periods of low/high bandwidth are common and more bandwidth is gained during high periods than is lost during low periods. 

As a result, the authors proposed a method called **quality-enhancing retransmission**. Low-quality real-time frames transmitted during periods of low bandwidth can be retransmitted at a higher bitrate during a later period of high bandwidth while the live streaming session is ongoing, thus improving the video quality for delayed, time-shifted viewers. Choosing the optimal tradeoff between the drop in real-time quality\(because of the retransmission\) and the improvement in delayed video quality can be envisioned as a quality \(SSIM\) maximization problem across the time-shifted delays



