# Index

* \*\*\*\*[**Serverless Computation with OpenLambda**](https://www.usenix.org/conference/hotcloud16/workshop-program/presentation/hendrickson) ****- ****Hendrickson et al., HotCloud' 16
* [**Encoding, Fast and Slow: Low-Latency Video Processing Using Thousands of Tiny Threads**](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-fouladi.pdf) - Fouladi et al., NSDI’ 17
  * Leverage the emerging microservice frameworks\(e.g., AWS Lambda\) to provide low-latency video processing
  * Key insight: Video encoding can be divided into fast and slow parts, with the “slow” work\(searching for correlations between frames\) done in parallel across thousands of tiny threads, and only “fast” work done serially.
  * Exploits the codec's ability to save and restore its internal state
* \*\*\*\*[**Occupy the Cloud: Distributed Computing for the 99%**](https://shivaram.org/publications/pywren-socc17.pdf) - Jonas et al., SoCC' 17
* \*\*\*\*[**SAND: Towards High-Performance Serverless Computing**](https://www.usenix.org/conference/atc18/presentation/akkus) ****- ****Akkus et al., ATC' 18
* \*\*\*\*[**Peeking Behind the Curtains of Serverless Platforms** ](https://www.usenix.org/conference/atc18/presentation/wang-liang)- Wang et al., ATC' 18
* \*\*\*\*[**SOCK: Rapid Task Provisioning with Serverless-Optimized Containers**](https://www.usenix.org/conference/atc18/presentation/oakes) ****- Oakes et al., ATC' 18
* [**Sprocket: A Serverless Video Processing Framework**](http://cseweb.ucsd.edu/~gmporter/papers/socc18-sprocket.pdf) - Ao et al., SoCC’ 18
  * Extend the idea of [ExCamera](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-fouladi.pdf) - enable users to build more complex pipelines
  * novel straggler mitigation strategy 
* \*\*\*\*[**Pocket: Elastic Ephemeral Storage for Serverless Analytics**](https://www.usenix.org/conference/osdi18/presentation/klimovic) ****- ****Klimovic et al., OSDI' 18
* \*\*\*\*[**Cloud Programming Simplified: A Berkeley View on Serverless Computing**](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2019/EECS-2019-3.pdf) ****- Jonas et al., Tech Report 
* \*\*\*\*[**Serverless Computing: One Step Forward, Two Steps Back** ](http://cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf)- Hellerstein et al., CIDR' 19
* \*\*\*\*[**Shuffling, Fast and Slow: Scalable Analytics on Serverless Infrastructure**](https://www.usenix.org/conference/nsdi19/presentation/pu) ****- Pu et al., NSDI' 19
* \*\*\*\*[**Cirrus: a Serverless Framework for End-to-end ML Workflows**](https://dl.acm.org/doi/10.1145/3357223.3362711) ****- ****Carreira et la., SoCC' 19
* \*\*\*\*[**From Laptop to Lambda: Outsourcing Everyday Jobs to Thousands of Transient Functional Containers**](https://www.usenix.org/conference/atc19/presentation/fouladi) ****- ****Fouladi et al., ATC' 19
* \*\*\*\*[**InfiniCache: Exploiting Ephemeral Serverless Functions to Build a Cost-Effective Memory Cache**](https://www.usenix.org/conference/fast20/presentation/wang-ao) ****- Wang et al., FAST' 20
* \*\*\*\*[**Serverless in the Wild: Characterizing and Optimizing the Serverless Workload at a Large Cloud Provider**](https://www.usenix.org/conference/atc20/presentation/shahrad) ****- ****Shahrad et al., ATC' 20
* \*\*\*\*[**Sequoia: Enabling Quality-of-Service in Serverless Computing**](https://dl.acm.org/doi/10.1145/3419111.3421306) ****- Tariq et al., SoCC' 20
* \*\*\*\*[**Kappa: A Programming Framework for Serverless Computing**](https://dl.acm.org/doi/10.1145/3419111.3421277) ****- Zhang et al., SoCC' 20

