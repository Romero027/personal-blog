# Short Summaries

### [Neural Adaptive Content-aware Internet Video Delivery](https://www.usenix.org/system/files/osdi18-yeo.pdf) - Yeo et al., OSDI' 18

The key idea of this paper is to use **super-resolution**, a technique that uses DNN to recover a high-resolution image from lower resolution images, on top of ABR to enhance client-side video quality. It will train a DNN for each video offline, exploiting DNN's inherent overfitting property to guarantee reliable and superior performance. 

When a client requests a video from CDN server, the server provides the DNN corresponding to the video.The client then applies the DNN to the received low-quality video chunks by utilizing its own computing power. 

