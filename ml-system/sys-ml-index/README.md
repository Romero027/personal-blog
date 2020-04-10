# Index

### Data Validation

* [Automating Large-Scale Data Quality Verification](http://www.vldb.org/pvldb/vol11/p1781-schelter.pdf) - Schelter et al., VLDB' 18
  * Presented a system for automating the verification of data quality at scale
* [Data Validation for Machine Learning](https://www.sysml.cc/doc/2019/167.pdf) - Breck et al., SysML' 19
  * Designed a system to monitor the quality of data fed into machine learning algorithm in Google

### Federated/Decentralized Learning

* [Towards Federated Learning at Scale: System Design](https://arxiv.org/abs/1902.01046) - Bonawitz et al., SysML' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/towards-federated-learning-at-scale-system-design)\]
  * Discussed the general architecture and protocol of federated learning in Google
* [Communication-Efficient Learning of Deep Networks from Decentralized Data](https://arxiv.org/pdf/1602.05629.pdf) - McMahan et al.,arxiv 2017 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/towards-federated-learning-at-scale-system-design)\]
  * Described the Federated averaging algorithm
* [The Non-IID Data Quagmire of Decentralized Machine Learning](https://arxiv.org/pdf/1910.00189.pdf) - Hsieh et al., arxiv 2019 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/learning-from-non-iid-data)\]
  * Studied the problem of non-IID data partition, and designed a system-level approach which adapts the communication frequency to reflect the skewness in the data.  
* \*\*\*\*[Federated Learning for Mobile Keyboard Prediction](https://arxiv.org/abs/1811.03604) - Hard et al., arxiv 2018 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/towards-federated-learning-at-scale-system-design)\]
  * Demonstrated the feasibility and benefit of training language models on client devices without exporting sensitive user data.
* [Federated Multi-Task Learning](https://arxiv.org/abs/1705.10467)\* - Smith et al., arxiv 2017 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/misc#cartel-a-system-for-collaborative-transfer-learning-at-the-edge-daga-et-al-2019)\]
  * Proposed that distributed multi-task learning is naturally suited to handle the statistical challenges\(e.g. Non-iid\) in FL.
* [Learning Differentially Private Recurrent Language Models](https://arxiv.org/abs/1710.06963) - McMahan et al., arxiv 2017 \[[Summary](https://xzhu0027.gitbook.io/blog/machine-learning/dl-fl-with-differential-privacy)\]

### Distributed Machine Learning

* [Large Scale Distributed Deep Networks](https://papers.nips.cc/paper/4687-large-scale-distributed-deep-networks) - Dean et al., NIPS' 12
  * Introduced the idea of Model Parallelism and Data Parallelism, as well as Google's first generation Deep Network platform, DistBelief. The ideas are "old", but it is a must read if you are interested in distributed Deep Network platforms. 
* [TensorFlow: A System for Large-Scale Machine Learning](http://download.tensorflow.org/paper/whitepaper2015.pdf) - Abadi et al., OSDI' 16
  * The core idea behind TensorFlow is the dataflow\(with mutable state\) representation of the Deep Networks, which they claim to subsume existing work on parameter servers, and offers a uniform programming model that allows users to harness large-scale heterogeneous systems, both for production tasks and for experimenting with new approaches.
* [Ray: A Distributed Framework for Emerging AI Applications](https://www.usenix.org/system/files/osdi18-moritz.pdf) - Moritz et al., OSDI' 18 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/ray-a-distributed-framework-for-emerging-ai-applications)\] 
* [Scaling Distributed Machine Learning with the Parameter Server](http://www.cs.cmu.edu/~muli/file/parameter_server_osdi14.pdf) - Li et al., OSDI' 14 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/parameter-servers)\]
  * Described the architecture and protocol of parameter server
* [More Effective Distributed ML via a Stale Synchronous Parallel Parameter Server](http://www.cs.cmu.edu/~seunghak/SSPTable_NIPS2013.pdf) - Ho et al., NIPS' 13 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/parameter-servers)\]
  * Discussed the Stale Synchronous Parallel\(SSP\) and an implementation of SSP parameter server
* [Project Adam: Building an Efficient and Scalable Deep Learning Training System](https://www.usenix.org/node/186213) - Chilimbi et al., OSDI' 14
  * Described the design and implementation of a distributed system called Adam comprised of commodity server machines to train deep neural networks.
* [Gaia: Geo-Distributed Machine Learning Approaching LAN Speeds](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-hsieh.pdf) - Hsieh et al., NSDI' 17 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/parameter-servers)\]
  * Designed a geo-distributed ML system which differentiates communication within data center and across data centers and presented the Approximate Synchronous Parallel model\(similar to SSP\).
* [Adaptive Communication Strategies in Local-Update SGD](https://arxiv.org/pdf/1810.08313.pdf) **-** Wang et al., SysML' 18 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/misc#adaptive-communication-strategies-in-local-update-sgd-wang-et-al-2018)\]
  * Proposed an adaptive algorithm for choosing $$\tau$$ in asynchronous training.\(Change $$\tau $$ as algorithm converges\)
* [Gradient Coding: Avoiding Stragglers in Distributed Learning](http://proceedings.mlr.press/v70/tandon17a.html)  **-** Tandon et al., ICML' 17 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/misc#gradient-coding-avoiding-stragglers-in-distributed-learning-tandon-et-al-2017)\]
  * Coded Computation
* [Revisiting Distributed Synchronous SGD](https://arxiv.org/pdf/1604.00981.pdf) - Chen et al., 2017 
  * Proposed a solution to mitigate stragglers: adding b extra workers, but as soon as the parameter servers receive gradients from any N workers, they stop waiting and update their parameters using the N gradients.
* [PipeDream: Generalized Pipeline Parallelism for DNN Training](https://cs.stanford.edu/~matei/papers/2019/sosp_pipedream.pdf) - Narayanan et al., SOSP' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/pipedream-generalized-pipeline-parallelism-for-dnn-training)\]
  * Proposed Pipeline-parallel training that combines data and model parallelism with pipelining. 
* \*\*\*\*[A Generic Communication Scheduler for Distributed DNN Training Acceleration](https://dl.acm.org/authorize?N695016) - Peng et al., SOSP' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/prediction-serving#nexus-a-gpu-cluster-engine-for-accelerating-dnn-based-video-analysis)\]
  * Key insight: Communication of former layers of a neural network has higher priority and can preempt communication of latter layers.

### Deep Learning Scheduler

* [Optimus: An Efficient Dynamic Resource Scheduler for Deep Learning Clusters](https://i.cs.hku.hk/~cwu/papers/yhpeng-eurosys18.pdf) - Peng et al., EuroSys' 18
* [Gandiva: Introspective Cluster Scheduling for Deep Learning](https://www.usenix.org/conference/osdi18/presentation/xiao) - Xiao et al., OSDI' 18 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/gandiva-introspective-cluster-scheduling-for-deep-learning)\]
* [Tiresias: A GPU Cluster Manager for Distributed Deep Learning](https://www.usenix.org/system/files/nsdi19-gu.pdf) - Gu et al., NSDI' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/tiresias-a-gpu-cluster-managerfor-distributed-deep-learning)\]
* [Themis: Fair and Efficient GPU Cluster Scheduling](https://www.usenix.org/conference/nsdi20/presentation/mahajan) - Mahajan et al., NSDI' 20 

### Prediction Serving Systems

* [Clipper: A Low-Latency Online Prediction Serving System](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-crankshaw.pdf) - Crankshaw et al., NSDI' 17 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/prediction-serving)\]
  * Discussed the challenge of prediction serving systems and presented their general-purpose low-latency prediction serving system.
* [Pretzel: Opening the Black Box of Machine Learning Prediction Serving Systems](https://www.usenix.org/system/files/osdi18-lee.pdf) - Lee et al., OSDI' 18
* [Parity Models: Erasure-Coded Resilience for Prediction Serving Systems](http://delivery.acm.org/10.1145/3360000/3359654/p30-kosaian.pdf?ip=35.3.50.157&id=3359654&acc=OPENTOC&key=93447E3B54F7D979%2E0A17827594E6F2C8%2E4D4702B0C3E38B35%2EC42B82B87617960C&__acm__=1572846710_212460fc2118b4ddbb56646253af114b) - Kosaian et al, SOSP' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/prediction-serving)\]
  * Learning based approach to achieve erasure coded resilience for Neural Networks.
* [Nexus: a GPU cluster engine for accelerating DNN-based video analysis](https://dl.acm.org/doi/10.1145/3341301.3359658) - Shen et al., SOSP' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/prediction-serving#nexus-a-gpu-cluster-engine-for-accelerating-dnn-based-video-analysis)\]

### Machine Learning Systems in industry

* Uber's Machine Learning Platform - Michelangelo: \[[Blog](https://eng.uber.com/michelangelo/) and [Video](https://www.youtube.com/watch?v=iCpp5mqTeXE)\]
* [Horovod: fast and easy distributed deep learning in TensorFlow](https://arxiv.org/pdf/1802.05799) - Sergeev et al., 2018 \[[Github](https://github.com/horovod/horovod)\]\[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/parameter-servers#parameter-server-vs-allreduce)\]
  * Horovod is Uber's distributed training framework for TensorFlow, Keras, PyTorch, and MXNet. The goal of Horovod is to make distributed Deep Learning fast and easy to use.
* [Machine Learning at Facebook: Understanding Inference at the Edge](https://research.fb.com/wp-content/uploads/2018/12/Machine-Learning-at-Facebook-Understanding-Inference-at-the-Edge.pdf) - Wu et al., 2018 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/misc#machine-learning-at-facebook-understanding-inference-at-the-edge-wu-et-al-2018)\]
  * Facebook's work on bringing machine learning inference to the edge. 

### Video Analytics

* [NoScope: Optimizing Neural Network Queries over Video at Scale](http://www.vldb.org/pvldb/vol10/p1586-kang.pdf) - Kang et al, VLDB' 17 \[[Summary](https://xzhu0027.gitbook.io/blog/video-analytics/untitled/noscope-optimizing-neural-network-queriesover-video-at-scale)\]
* [Live Video Analytics at Scale with Approximation and Delay-Tolerance](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-zhang.pdf) - Zhang et al., NSDI17' \[[Summary](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-zhang.pdf)\]
* [Chameleon: Scalable Adaptation of Video Analytics via Temporal and Cross-camera Correlations](https://people.cs.uchicago.edu/~junchenj/docs/Chameleon_SIGCOMM_CameraReady_faceblurred.pdf) - Jiang et al., SIGCOMM' 18 \[[Summary](https://xzhu0027.gitbook.io/blog/video-analytics/untitled/chameleon-scalable-adaptation-of-video-analytics)\]
* [Focus: Querying Large Video Datasets with Low Latency and Low Cost](https://www.usenix.org/system/files/osdi18-hsieh.pdf) - Hsieh et al., OSDI' 18 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/focus-querying-large-video-datasetswith-low-latency-and-low-cost)\]
  * Presented Focus, a system provide low-cost and low-latency querying on large historical video datasets 
* [Scaling Video Analytics on Constrained Edge Nodes](https://arxiv.org/abs/1905.13536) - Canel et al., arxiv 2019 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/untitled)\]
  * Introduced FilterForward, an edge-to-cloud system that identifies the video sequences that are relevant to datacenter applications and offloads only that data for further analysis. 

### Misc.

* \*\*\*\*[Cartel: A System for Collaborative Transfer Learning at the Edge](https://dl.acm.org/citation.cfm?id=3362708) - Daga et al., SoCC' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/misc#cartel-a-system-for-collaborative-transfer-learning-at-the-edge-daga-et-al-2019)\]
  * Proposed a framework for Collaborative Learning
* [Collaborative Learning between Cloud and End Devices: An Empirical Study on Location Prediction](https://www.microsoft.com/en-us/research/uploads/prod/2019/08/sec19colla.pdf) - Lu et al., SEC' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/misc#collaborative-learning-between-cloud-and-end-devices-an-empirical-study-on-location-prediction-lu-et-al-2019)\]
* [DeepXplore: Automated Whitebox Testing of Deep Learning Systems](http://www.cs.columbia.edu/~junfeng/papers/deepxplore-sosp17.pdf) - Pei et al., SOSP' 17 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/deepxplore-automated-whitebox-testingof-deep-learning-systems)\]
* [Analysis of Large-Scale Multi-Tenant GPU Clusters for DNN Training Workloads](https://www.usenix.org/conference/atc19/presentation/jeon) - Jeon et al., ATC' 19 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/sys-ml-index/misc-1#analysis-of-large-scale-multi-tenant-gpu-clusters-for-dnn-training-workloads)\]
* [A unifying view on dataset shift in classification](https://rtg.cis.upenn.edu/cis700-2019/papers/dataset-shift/dataset-shift-terminology.pdf) - Moreno-Torres et al., 2010 
  * Explained various types of data shift\(i.e. covariate shift, . prior probability shift and Concept Shift\) 

\*denotes papers that I plan to read



