# Short Summaries

### [AdaptSize: Orchestrating the Hot Object Memory Cache in a Content Delivery Network](https://www.cs.cmu.edu/~harchol/Papers/NSDI17.pdf) - Berger et al., NSDI' 17

This paper inspects the caching mechanism of Content Delivery Networks\(CDNs\). A CDN server typically employs two levels of caching: a small but fast in-memory cache called Hot Object Cache\(HOC\) and a large second-level Disk Cache\(DC\). The goal of AdaptSize is to maximize the object hit ratio of HOC. 

The key insights are 1. the HOC is subject to extreme variability in request patterns and object size\( up to 9x difference\). As a result, not all objects should be admitted to the HOC and 2. existing works only focus on cache eviction and assume objects are of the same size. Based on these observations, the authors proposes AdaptSize, which is an near-optimal method for size-aware cache admission. AdaptSize admits objects with probability $$e^{-size/c}$$ and evicts objects using a concurrent [variant](https://varnish-cache.org/trac/wiki/ArchitectureLRU) of LRU. As the optimal c changes over time, AdaptSize uses a Markov chain model to find the best c.



