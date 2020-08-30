# Short Summaries

### [Neural Adaptive Content-aware Internet Video Delivery](https://www.usenix.org/system/files/osdi18-yeo.pdf) - Yeo et al., OSDI' 18

The key idea of this paper is to use **super-resolution**, a technique that uses DNN to recover a high-resolution image from lower resolution images, on top of ABR to enhance client-side video quality. It will train a DNN for each video offline, exploiting DNN's inherent overfitting property to guarantee reliable and superior performance. 

When a client requests a video from CDN server, the server provides the DNN corresponding to the video. The client then applies the DNN to the received low-quality video chunks by utilizing its own computing power. 

### \*\*\*\*[**Neural-Enhanced Live Streaming: Improving Live Video Ingest via Online Learning**](http://ina.kaist.ac.kr/~livenas/livenas_sigcomm2020.pdf) - Kim et al., SIGCOMM' 20

The key observation is that the stream quality is fundamentally constrained by the streamer's uplink bandwidth and its computational capacity. Extending the idea of the above paper, this paper presents **LiveNAS**, a neural-enhanced live video streaming system, which breaks the strong dependency between the quality of live video and the ingest client's bandwidth.

Since it focuses on live streaming applications, pre-trained networks are not possible. Instead, LiveNAS trains super-resolution DNNs via online learning. Besides the encoded video, the client transmits small patches of high-quality raw frames which will serve as ground truth for training. Note that even a fraction of ground truth labels can provide substantial training gains because of video's temporal redundancy.





