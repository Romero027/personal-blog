# Index

### System

* [**Glimpse: Continuous, Real-Time Object Recognition on Mobile Devices**](http://people.csail.mit.edu/yuhan/doc/sen060-chenA.pdf) - Chen et al., SenSys’ 15
  * Cache the results to hide the network delivery and server processing latency
  * Only send the frames that are largely different from the previous frames
* [**Starfish: Efficient Concurrency Support for Computer Vision Applications**](https://dl.acm.org/doi/pdf/10.1145/2742647.2742663) - LiKamWa et al., MobiSys’ 15
  * Track identical library calls and reuse computed results across multiple applications
* [**The Design and Implementation of a Wireless Video Surveillance System**](https://www.microsoft.com/en-us/research/wp-content/uploads/2017/08/Bahl-MobiCom-2015.pdf) - Zhang et al., MobiCom’ 15 
  * Group cameras monitoring the same area into clusters
  * Uploads frames with high “utility”\(e.g., object count\)
  * Uploads frames that are different from previous frames\(e.g., different object counts\)
* [**MCDNN: An Approximation-Based Execution Framework for Deep Stream Processing Under Resource Constraints**](https://homes.cs.washington.edu/~arvind/papers/mcdnn.pdf)  - Han et al., MobiSys’ 16
  * Adaptively pick the best specialized model 
* \*\*\*\*[**Optasia: A Relational Platform for Efficient Large-Scale Video Analytics**](https://www.microsoft.com/en-us/research/wp-content/uploads/2017/01/optasia_socc16.pdf) ****-  Lu et al., SOCC' 16
* [**DeepEye: Resource Efficient Local Execution of Multiple Deep Vision Models using Wearable Commodity Hardware**](https://dl.acm.org/doi/10.1145/3081333.3081359) **-** Mathut et al., MobiSys’ 17
  * A system that can run multiple cloud-scale DL models locally on wearable devices
  * Interleave the loading of memory-intensive FC layers and the execution of compute-intensive convolution layers
* [**Fast Video Classification via Adaptive Cascading of Deep Models**](http://openaccess.thecvf.com/content_cvpr_2017/papers/Shen_Fast_Video_Classification_CVPR_2017_paper.pdf) - Shen et al., CVPR’ 17
  * Leverage the short-term class skew using model cascade
  * Train specialized video online
* [**NoScope: Optimizing Neural Network Queries over Video at Scale**](https://arxiv.org/abs/1703.02529)  - Kang et al., VLDB’ 17
  * Model cascade: difference detector\(MSE\) → cheap/specialized model → full model
* [**Live Video Analytics at Scale with Approximation and Delay-Tolerance**](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-zhang.pdf) - Zhang et al., NSDI’ 17
  * Objective: support efficient real-time analytics for multiple queries which have different quality and lag goals
  * Offline Phase: use profiler to get a set of pareto-optimal configurations\(a combination of knobs\) from resource-quality space \(with a variant of greedy hill climbing\)
  * Online Phase: periodically change running queries’ configurations/placement/resource allocation to maximize total utility\(quality + lag goals\) 
* [**Cachier: Edge-Caching for Recognition Applications**](https://ieeexplore.ieee.org/document/7979974) - Drolia et al., 2017
  * Use edge server as a cache with compute resources\(smiliar to CDN\)
* [**Encoding, Fast and Slow: Low-Latency Video Processing Using Thousands of Tiny Threads**](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-fouladi.pdf) - Fouladi et al., NSDI’ 17
  * Leverage the emerging microservice frameworks\(e.g., AWS Lambda\) to provide low-latency video processing
  * Key insight: Video encoding can be divided into fast and slow parts, with the “slow” work\(searching for correlations between frames\) done in parallel across thousands of tiny threads, and only “fast” work done serially.
  * Exploits its codec's ability to save and restore its internal state
* [**Scanner: Efficient Video Analysis at Scale**](https://arxiv.org/abs/1805.07339) - Poms et al., SIGGRAPH’ 18
  * Store videos as tables which are optimized for frame sampling on compressed videos
  * Express frame operations as dataflow graphs
* [**Mainstream: Dynamic Stem-Sharing for Multi-Tenant Video Processing**](https://www.usenix.org/system/files/conference/atc18/atc18-jiang.pdf) - Jiang et al., ATC’ 18
  * Transfer learning → execute common layers only once
  * Processing more frames with shared DNN vs. greater per-frame accuracy with specialized DNN
* [**Chameleon: Scalable Adaptation of Video Analytics**](https://people.cs.uchicago.edu/~junchenj/docs/Chameleon_SIGCOMM_CameraReady_faceblurred.pdf) - Jiang et al., SIGCOMM’ 18
  * Resource-accuracy tradeoff is affected by some persistent characteristics, so we can reuse configurations over time → temporal correlation
  * Video cameras with same characteristics share same best configurations → cross-camera correlations
  * Configuration knobs independently impact accuracy → reduce search space
  * Divide cameras into groups → periodically re-profile “leader” videos 
* [**AWStream: Adaptive Wide-Area Streaming Analytics**](https://awstream.github.io/paper/awstream.pdf) - Zhang et al., SIGCOMM’ 18
  * Objective: low latency and high accuracy stream processing in WAN
  * Ask programmers to write degradation functions and profile those configurations
  * Adaptively change configuration at runtime → react to congestions
* [**Focus: Querying Large Video Datasets with Low Latency and Low Cost**](https://www.usenix.org/conference/osdi18/presentation/hsieh) - Hsieh et al., OSDI’ 18
  * Enable low-latency and low-cost querying over large historical video datasets.
  * At ingest time: classify objects using a cheap CNN, cluster similar objects\(KNN search\), and index each cluster using top-K most confident classification results.
  * At query-time: looks up the ingest index for cluster centroids that match the class and classifies them using expensive CNN. 
* [**Salsify: Low-Latency Network Video through Tighter Integration between a Video Codec and a Transport Protocol**](https://cs.stanford.edu/~keithw/salsify-paper.pdf) - Fouladi et al., NSDI’ 18
  * Proposes a tightly coupled codec and transport protocol
  * Exploits its codec's ability to save and restore its internal state
  * Three options when sending the next frame\(lower quality frame/higher quality frame/skip\)
  * Never send a frame unless the network is ready
* [**On-Demand Deep Model Compression for Mobile Devices: A Usage-Driven Model Selection Framework**](https://tik-old.ee.ethz.ch/file//79a7dd6f6370f809e6180c0746232283/mobisys18-liu.pdf) - Liu et al., MobiSys’ 18
  * Adaptively select DNN compression techniques based on user demand\(Acc/Storage/Comp cost/Latency/Energy\) 
* [**Sprocket: A Serverless Video Processing Framework**](http://cseweb.ucsd.edu/~gmporter/papers/socc18-sprocket.pdf) - Ao et al., SoCC’ 18
  * Extend the idea of [ExCamera](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-fouladi.pdf) - enable users to build more complex pipelines
  * novel straggler mitigation strategy 
* \*\*\*\*[**Potluck: Cross-Application Approximate Deduplication for Computation-Intensive Mobile Applications**](https://www.cs.yale.edu/homes/guo-peizhen/files/potluck-asplos18.pdf) ****- Guo et al., ASPLOS' 18
* \*\*\*\*[**ReXCam: Resource-Efficient, Cross-Camera Video Analytics at Scale**](https://arxiv.org/abs/1811.01268) ****- Jain et al., arXiv' 18
* [**DeepLens: Towards a Visual Data Management System**](http://cidrdb.org/cidr2019/papers/p40-krishnan-cidr19.pdf) - Krishnan et al., CIDR’ 19
  * Objective: Indexing and query optimization for VDMS\(For complex queries like join\)
  * Novel model for encoding, indexing and storing lineage
* [**Networked Cameras Are the New Big Data Clusters**](https://www.microsoft.com/en-us/research/uploads/prod/2019/08/hotedgevideo19camera.pdf) - Jiang et al., HotEdgeVideo’ 19
  * Proposes a new “camera cluster” abstraction
    * Saving computing resource
    * Resource Pooling
    * Improving analytics quality 
    * Hiding low-level intricacies
* [**Cracking open the DNN black-box: Video Analytics with DNNs across the Camera-Cloud Boundary**](https://dl.acm.org/doi/abs/10.1145/3349614.3356023) - Emmons et al., HotEdgeVideo 19
  * Split-brain inference
* [**Scaling Video Analytics on Constrained Edge Nodes**](https://arxiv.org/abs/1905.13536) - Canel et al., SysML’ 19
  * Assumption: relevant events are rare.
  * Filter frames by using a micro, binary classifier that extract feature maps from base DNN
* [**Bridging the Edge-Cloud Barrier for Real-time Advanced Vision Analytics**](https://www.usenix.org/conference/hotcloud19/presentation/wang) - Wang et al., HotCloud 19
  * Use super-resolution to enhance video quality before running analytics\(related: [NAS](https://www.usenix.org/system/files/osdi18-yeo.pdf)\)
* [**AdaScale: Towards Real-time Video Object Detection Using Adaptive Scaling**](https://arxiv.org/pdf/1902.02910.pdf) - Chin et al., SysML’ 19
  * Down-sampling images are sometimes beneficial in terms of accuracy\(e.g., removing background noise\)
  * Adaptively scaling video to improve both speed and accuracy of object detectors
* [**Edge Assisted Real-time Object Detection for Mobile Augmented Reality**](http://www.winlab.rutgers.edu/~luyang/papers/mobicom19_augmented_reality.pdf) - Liu et al., MobiCom’ 19
  * Dynamic RoI Encoding: decrease the encoding quality of uninterested areas\(use the last processed frame as heuristic\)
  * \(Dependency-aware\) Parallel streaming and inference: divide frames into slices and parallelize the processing between slices
* [**Scaling Video Analytics Systems to Large Camera Deployments**](https://rtcl.eecs.umich.edu/yuanchao/paper/hotmobile19video.pdf) - Jain et al., HotMobile’ 19
  * Leverage cross-camera correlations to reduce resource usage and achieve higher inference accuracy
* [**Visual Road: A Video Data Management Benchmark**](https://db.cs.washington.edu/projects/visualroad/p300-haynes.pdf) - Haynes et al., SIGMOD’ 19
  * An auto-generated benchmark that evaluates the performance of VDBMS
  * Let users place an arbitrary number of cameras, each with configurable position, resolution, and field of view
  * Composite queries and automatically generated ground truth labels
* \*\*\*\*[**Rekall: Specifying Video Events using Compositions of Spatiotemporal Labels**](https://arxiv.org/abs/1910.02993) ****- Fu et al., arXiv' 19
* [**BlazeIt: Optimizing Declarative Aggregation and Limit Queries for Neural Network-Based Video Analytics**](https://cs.stanford.edu/~matei/papers/2020/vldb_blazeit.pdf) - Kang et al., VLDB’ 20
  * Objective: Support \(approximate\) aggregate and limit queries over large video dataset
  * At ingest time, run object detection on small samples of frames and store them
  * For each query, use them to train a query-specific proxy model
* \*\*\*\*[**MIRIS: Fast Object Track Queries in Video** ](https://favyen.com/miris-sigmod.pdf)- Bastani et al., SIGMOD' 20
* \*\*\*\*[**Server-Driven Video Streaming for Deep Learning Inference**](https://dl.acm.org/doi/pdf/10.1145/3387514.3405887) ****- Du et al., SIGCOMM' 20
* \*\*\*\*[**Reducto: On-Camera Filtering for Resource-Efficient Real-Time Video Analytics**](https://dl.acm.org/doi/pdf/10.1145/3387514.3405874) - Li et al., SIGCOMM' 20

\*\*\*\*

### Algorithm

#### Efficient image/video processing

* \*\*\*\*[**End-to-end Learning of Action Detection from Frame Glimpses in Videos**](https://arxiv.org/pdf/1511.06984.pdf) ****- Yeung et al., CVPR' 16
* **Watching a Small Portion could be as Good as Watching All: Towards Efficient Video Classification** - Fan et al., IJCAI’ 18
* **AdaFrame: Adaptive Frame Selection for Fast Video Recognition** - Wu et al., CVPR’ 19
* **Multi-Agent Reinforcement Learning Based Frame Sampling for Effective Untrimmed Video Recognition** - Wu et al., ICCV’ 19
* **Listen to Look: Action Recognition by Previewing Audio** - Gao et al., CVPR’ 2020

### 



