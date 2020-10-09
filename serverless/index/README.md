# Index

* [**Encoding, Fast and Slow: Low-Latency Video Processing Using Thousands of Tiny Threads**](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-fouladi.pdf) - Fouladi et al., NSDI’ 17
  * Leverage the emerging microservice frameworks\(e.g., AWS Lambda\) to provide low-latency video processing
  * Key insight: Video encoding can be divided into fast and slow parts, with the “slow” work\(searching for correlations between frames\) done in parallel across thousands of tiny threads, and only “fast” work done serially.
  * Exploits the codec's ability to save and restore its internal state
* [**Sprocket: A Serverless Video Processing Framework**](http://cseweb.ucsd.edu/~gmporter/papers/socc18-sprocket.pdf) - Ao et al., SoCC’ 18
  * Extend the idea of [ExCamera](https://www.usenix.org/system/files/conference/nsdi17/nsdi17-fouladi.pdf) - enable users to build more complex pipelines
  * novel straggler mitigation strategy 
* \*\*\*\*[**Serverless Computing: One Step Forward, Two Steps Back** ](http://cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf)- Hellerstein et al., CIDR' 19

