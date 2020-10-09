# Short Summaries

### \*\*\*\*[**Potluck: Cross-Application Approximate Deduplication for Computation-Intensive Mobile Applications**](https://www.cs.yale.edu/homes/guo-peizhen/files/potluck-asplos18.pdf) ****- Guo et al., ASPLOS' 18

This paper proposes a caching mechanism to enable cross-application approximate deduplication. The feature vector of input\(e.g, a frame\) serves as the key. Thus, a lookup attempt is made with the key and the name of the function called by matching the input key to any existing key within a given similarity threshold. To strike the trade-off between performance speedup from reusing previous results and the accuracy of the results, Potluck adaptively tunes the similarity threshold. One thing to note is that Potluck only saves the result of the model and does not save any intermediate results within a model.



### [DeepCache: Principled Cache for Mobile Deep Vision](https://dl.acm.org/doi/10.1145/3241539.3241563) - Xu et al., MobiCom' 18

This paper aims to cache CNN's final results as well as its intermediate results, exploiting the temporal locality among a video stream. In other words, consecutive video frames often have substantial similar overlapped regions.

For a CNN model, DeepCache maintains a cache, covering the model’s input as well as its internal layers. The cache stores recent video frames for the model input, and recent feature maps for the internal layers. The cache keys are equal-sized, fine-grained regions on the cached input frames. The cache values are the cached feature maps produced by the layers.

Note that, Deep cache is inspired by video compression techniques and aims to cache results for a single application.

### 



