## Machine Learning (In) Security: A Stream of Problems Part 2 (Seminar 2.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

### Presentation summary
#### concept drift
- situation in which the relation between input data and target variable changes over time
  - attackers advance malware all the time
- concept evoluation is the process of defining and refining concepts, producing new labels
- types of concept drift
  - sudden: creating a completely new attack
  - recurring: old attacks appear again after some time
  - gradual: new attacks created......over time?
  - incremental: slowly improving attacks?

#### solutions to concept drift
- ensemble of classifiers
- use temporal similarty (cosine or jaccard), if behavior today is similar to the past
- meta features: summarizing features rather than looking at each feature individually
- online learning
- when models tend to become obsolete (venn abers predictors which looks at when models begin to fade, transcend)
- reinforcement learning to generate adversarial samples and retrain
- automate feature set and models update (droidevolver)
- DDM
  - drift detection method
  - can define a warning and drift levels
  - keep track of error rate
- EDDM
  - early drift detection method
- ADWIN
- delayed labeling (unsupervised)
- learn and adapt by paying attention to what went wrong in the past
- monitor distribution of error

#### adversarial attacks
- manipulating a data point to make it seem like the other class (tricking the model)
- can be done by appending malware to benign binaries, tricking PE to be biased towards detection of packers instead of learning concept of a malicious binary

#### Direct attacks
- white box
  - attacker has full access to the model
  - gradient based attacks
  - craft deceptive data
- black box
  - attacker only has access to inputs and outputs of models
  - create random perturbations and trial and error within the model
  - change characteristics
  - try to mimic

#### solutions to direct attacks
- generative adversarial networks
- generate an algorithm that automatically generates some adversarial samples (data augmentation or oversampling)
- using probability labels instead of hard class labels
- use only non-negative weights (non negative malconv)

#### class imbalance solutions
- cost-sensitive learning (fast but hard to implement)
- ensemble learning
  - bagging (random forest)
  - bossting (adaboost)
- anomaly detection (SVM and isolation forest) to create decision boundaries

#### Transfer learning problem
- transfer learning is great but they have problems
- they are public (attackers have full access)
- they can produce adversarial vectors which affect both models (including base model)

#### transfer learning solutions
- use robust base models

#### implementation problem
- popular frameworks like sklearn and weka dont work well in cyber bc they process data in batches (as opposed to stream)
- multi-language codebases
- optimization
- implementing data stream algo pipelines (have to implement the whole pipeline, if a part of that fails the whole thing does)
- performance

#### possible solutions to implementation problem
- scikit-multiflow
- massive online analysis (MOA)
- river
- spark streaming
- adversarial ml frameworks
- most ml implementations are optimized by C and C++ under the hood
- outsource processing of ml algos to third party components

#### Evaluation
- common classification metrics
  - accuracy (can provide wrong conclusions, don't use on imbalanced dataset)
  - confusion matrix
  - recall
  - precision
  - F1 score
- evaluation frameworks
  - conformal evaluator
  - tesseract

#### evaluation pitfalls
- comparing apples and oranges
- delayed labels evaluation

#### understanding ML
- no such thing as a 0-day detector
- security and explainable (explainability great for teams to perform root cause analysis)
- arms race will always exist

#### future of ml in cyber
- researchers want to broaden adversarial attacks to broaden ML

### discussion summary
- concept drift: malware changes (or drifts) overtime, regardless of whether you handle it or not. it's a phenomenon
- can combat that by using detectors to see if the phenomenon is happening then handle the problem from there
- aggressive detector: constantly changes model, always have an updated model (can detect so early that a drift isnt actually happening)
- non-aggressive detector: detects the drift too late
- need to find a balance
- imbalanced datasets solutions
  - resampling (undersampling oversampling)
  - more powerful classifier (deep learning)
  - adapt the classifier to learn that the weights of classes are different (cost-sensitive learning) (if you don't want to resample)
    - adjust loss function to make model learn the weights
- random forest is the most common (or best in ml for cyber): it draws a non-linear decision boundary, it is the most adaptive model because of that
- there exists ensemble of ensemble (random forest with SVM where sometimes one classifies, another time another model classifies, or there is a majority vote for which model to use)
- ex: random forest on the user device and neural network on the cloud (since user device is going to be updated more frequently)
- hierarchical clustering: rather than having many classes (like a million malware families) you cluster a subset based on families that represent the million well. features are used as you go down the tree?
- a way to improve performance and works for outlier detection is one-class classifier: is this data point in this class or is it one of the other million classes? don't care if it is a part of the others
- 0-day (look up difference between signature and machine learning)
