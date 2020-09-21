# Index

### Algorithm

* [A Few Useful Things to Know About Machine Learning](https://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf) - Domingos CACM' 12 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/ml)\]
* [Hidden Technical Debt in Machine Learning Systems](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf) - Sculley et al, NIPS' 15 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/hidden-technical-debt-in-machine-learning-systems)\]
  * Identified the technical debt of Machine Learning system and provided some common mitigation strategy.
* [Don't Decay The Learning Rate, Increase The Batch Size](https://openreview.net/pdf?id=B1Yy1BxCZ)\* - Smith et al, ICLR' 18 
  * Showed that we can increase the batch size, instead of decreasing the learning rate, to get equivalent test accuracies after the same number of training epochs, but with fewer parameter updates, leading to greater parallelism and shorter training times.
* [Accelerating Deep Learning by Focusing on the Biggest Losers](https://arxiv.org/abs/1910.00762) - Jiang et al., arxiv 2019 \[[Summary](https://xzhu0027.gitbook.io/blog/ml-system/accelerating-deep-learning-by-focusing-on-the-biggest-losers)\]
  * Proposes an algorithm that accelerates the training of deep neural networks \(DNNs\) by prioritizing examples with high loss at each iteration. 



### Faster Inference

* \*\*\*\*[**End-to-end Learning of Action Detection from Frame Glimpses in Videos**](https://arxiv.org/pdf/1511.06984.pdf) ****- Yeung et al., CVPR' 16
* \*\*\*\*[**Watching a Small Portion could be as Good as Watching All: Towards Efficient Video Classification**](https://www.ijcai.org/Proceedings/2018/0098.pdf) ****- Fan et al., IJCAI’ 18
* \*\*\*\*[**AdaFrame: Adaptive Frame Selection for Fast Video Recognition**](https://arxiv.org/abs/1811.12432) ****- Wu et al., CVPR’ 19
* \*\*\*\*[**Multi-Agent Reinforcement Learning Based Frame Sampling for Effective Untrimmed Video Recognition**](https://arxiv.org/abs/1907.13369) ****- Wu et al., ICCV’ 19
* \*\*\*\*[**Listen to Look: Action Recognition by Previewing Audio**](https://arxiv.org/abs/1912.04487) ****- Gao et al., CVPR’ 2020
* \*\*\*\*[**FastBERT: a Self-distilling BERT with Adaptive Inference Time**](https://www.aclweb.org/anthology/2020.acl-main.537.pdf) - Liu et al., ACL' 2020

### Security/Privacy 

* [Deep Learning with Differential Privacy](https://arxiv.org/pdf/1607.00133.pdf) - Abadi et al., CCS' 16 \[[Summary](https://xzhu0027.gitbook.io/blog/machine-learning/dl-fl-with-differential-privacy)\]
  * Discussed how to train deep neural networks with non-convex objectives, under a modest privacy budget
* [Deep Models Under the GAN: Information Leakage from Collaborative Deep Learning](https://arxiv.org/abs/1702.07464)\* - Hitaj et al., CCS' 17
  * Proposed and implement an active inference attack on deep neural networks in a collaborative setting\(which stresses the importance of using secure aggregation and differential privacy.\)

