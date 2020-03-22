# Hidden Technical Debt in Machine Learning Systems

Developing and deploying ML systems is relatively fast and cheap, but maintaining them over time is difficult and expensive. This paper identifies the technical debts that are special to ML systems and provides some common mitigation strategies. 

### Complex models Erode Boundaries

**Abstraction** is arguably the most powerful tool we have to cope with complexity. Strict abstraction boundaries help express the invariants and logical consistency of information inputs and outputs from an given component. For ML systems, the desired behavior cannot be effectively expressed in software logic without dependency on external data. 

#### Entanglement

Machine learning systems mix signals together, entangling them and making isolation of improvements impossible. For instance, consider a system that uses features x1, ...xn in a model. If we change the input distribution of values in x1, the importance, weights, or use of the remaining n − 1 features may all change. 

> Changing Anything Changes Everything \(CACE principle\)

No inputs are ever really independent. Even worse, CACE applies not only to input signals, but also to hyper-parameters, learning settings, sampling methods, convergence thresholds, data selection, and essentially every other possible tweak

Mitigation strategies: 1. Isolate models and serve ensemble and 2. Detect changes in prediction behavior as they occur

#### Correction Cascades

In some cases, it is tempting to solve a learn a model M' that takes a model M which solves a similar problem as input and learns a small correction as fast way to solve the problem.

However, this correction model has created a new system dependency on M, making it significantly more expensive to analyze improvements to that model in the future. Once we have model built on top of each other, a correction cascade can create an improvement deadlock, as improving the accuracy of any individual component actually leads to system-level detriments.

Mitigation strategy: 1. Add features to distinguish among cases. and 2. Accept the cost of creating a separate model. 

#### Undeclared Consumers

Without access controls, some of these consumers may be undeclared, silently using the output of a given model as an input to another system. Undeclared consumers are expensive at best and dangerous at worst, because they create a hidden tight coupling of model M to other parts of the stack. Changes to ma will very likely impact these other parts, potentially in ways that are unintended, poorly understood, and detrimental.

Mitigation strategies: 1. Access restrictions and 2. Strict service-level agreement\(SLAs\).

### Data Dependencies Cost More than Code Dependencies

[Dependency debt](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/37755.pdf) is noted as key contributor to system complexity and technical debt. Code dependencies can be identified via static analysis by compilers and linkers. However, data dependencies in ML systems are much more difficult to detect. 

#### Unstable Data Dependencies

To move quickly, it is often convenient to consume signals as input features that are produced by other systems. However, in most cases, the input signals are unstable, especially they come from another ML model. This is dangerous because even “improvements” to input signals may have arbitrary detrimental effects in the consuming system that are costly to diagnose and address. 

Mitigation strategy: Versioned copy - Create a frozen version of the feature mapping and use it until such a time as an updated version has been fully vetted.

#### Underutilized data dependencies

Underutilized data dependencies are input signals that provide little incremental modeling benefit. These can make an ML system unnecessarily vulnerable to change, sometimes catastrophically so, even though they could be removed with no detriment

To cope with this issue, we should run exhaustive leave-one-feature-out evaluations regularly to identify and remove unnecessary features.

### Feedback loops

One of the key features of live ML systems is that they often end up influencing their own behavior if they update over time. This leads to a form of analysis debt, in which it is difficult to predict the behavior of a given model before it is released.

* **Direct Feedback Loops**: A model may directly influence the selection of its own future training data.
* **Hidden Feedback Loops**. A more difficult case is hidden feedback loops, in which two systems influence each other indirectly through the world. Improvements \(or, more scarily, bugs\) in one may influence the bidding and buying behavior of the other.

### ML-System Anti-Patterns

![](../../.gitbook/assets/screen-shot-2019-10-20-at-11.14.10-pm.png)

 It may be surprising to the academic community to know that only a tiny fraction of the code in many ML systems is actually devoted to learning or prediction.

#### Glue code 

Using generic packages often results in a glue code system design pattern, in which a massive amount of supporting code is written to get data into and out of general-purpose packages. Glue code is costly in the long term because it tends to freeze a system to the peculiarities of a specific package; testing alternatives may become prohibitively expensive

An important strategy for combating glue-code is to wrap black-box packages into common API’s. This allows supporting infrastructure to be more reusable and reduces the cost of changing packages.

There is a distinct lack of strong and widely accepted abstraction to support ML systems, or even distributed systems. One could argue that widespread use of Map-Reduce in machine learning was driven by the void of strong distributed learning abstractions. Indeed, one of the few areas of broad agreement in recent years appears to be that MapReduce is a poor abstraction for iterative ML algorithms. The lack of standard abstractions makes it all too easy to blur the lines between components.



#### Dealing with Changes in the External World

One of the things that makes ML systems so fascinating is that they often interact directly with the external world. 

* **Fixed Thresholds in Dynamic Systems**: It is often necessary to pick a decision threshold for a given model to perform some action, but such thresholds are often manually set. Thus if a model updates on new data, the old manually set threshold may be invalid. 
* **Monitoring and Testing:** Comprehensive live monitoring of system behavior in real time combined with automated response is critical for long-term system reliability



### Conclusion

When developing a new ML system, we want to make additional efforts in the areas of maintainable ML, including better abstractions, testing methodologies, and design patterns.









