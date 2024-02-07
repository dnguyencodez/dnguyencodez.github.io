## Machine Learning (In) Security: A Stream of Problems Part 2 (Seminar 2.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Ayushri Jain presented information from sections 6, 7, and 8 of [Machine Learning (In) Security: A Stream of Problems](https://arxiv.org/pdf/2010.16045.pdf). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information discussed by both Akshats as well as a summary of our class discussion.

### Presentation summary

---
### Review From the Last Seminar
- This seminar is a continuation of the paper discussed previously
- The last seminar covered the pitfalls and solutions of the first half of the ML process in cybersecurity
- Data collection
  - Use timestamps to combat the data leakage issue
  - Use data labels from a specific AV or find a way to unify all AV labels
  - Consider the changing nature of labels
  - Utilize resampling class imbalance, keep in mind the distributions
  - Experiment with the dataset size, the whole dataset does not typically need to be used
- Attribute extraction
  - Two general attribute extraction procedures: static and dynamic
  - Static attributes are file characteristics that can be observed without executing the file
  - Dynamic attributes are the characteristics and behaviors observed when malware is executed in a controlled environment such as a sandbox
  - In the experiments, models with dynamic attributes performed better overall
- Feature extraction
  - Feature extractors must constantly be updated due to malware evolution
  - Creating robust features is essential in defending against adversarial machine learning

#### Concept Drift
- The situation in which the relation between input data and target variable changes over time
  - Caused by an arms race between attackers and defenders
  - Attackers are constantly changing their attack vectors to bypass defenses
- Concept evolution is the process of defining and refining concepts, producing new labels to the underlying concepts
  - There could be a connection between concept evolution and drift in cybersecurity
  - New concepts results in new labels such as attackers produce new attacks
- Types of concept drift
  - sudden: creating a completely new attack (a concept is suddenly replaced by a new one)
  - recurring: old attacks appear again after some time
  - gradual: new attacks gradually replace old attacks over time
  - incremental: Attackers make few and small modifications to their attacks, but change over a long period of time

#### Possible Solutions of Concept Drift
- Ensemble of classifiers
  - Trained from consecutive sequences of data using v-fold partitioning
  - More resistant to changes
- Use temporal similarty (cosine or jaccard), if behavior today is similar to the past
  - Can help in determining the direction of the drift
- Meta features: summarizing features rather than looking at each feature individually
- Online learning using DroidOL which adapts to malware drift by updating models more aggressively when the error is larger
- Identify when models tend to become obsolete (Venn Abers predictors which looks at when models begin to fade)
- Use reinforcement learning to generate adversarial samples
  - Makes it possible to retrain a model
  - Hardens a model against worst-case inputs, protecting against concept drift
- Automate feature set and models update (DroidEvolver)
- Utilize concept drift detectors
  - There are supervised drift detectors that account for ground truth labels and unsupervised ones that do not
  - Supervised drift detectors: Drift Detection Method (DDM), Early Drift Detection Method (EDDM), and ADaptive WINdowing (ADWIN)
    - DDM and EDDM are based on sequential error (increase in consecutive error rate suggests concept drift)
    - ADWIN uses sliding windows and a threshold to determine if concept drift is occurring
  - Unsupervised: delayed labeling
    - Do not rely on the real label of samples and are useful when delays are expected

#### Adversarial attacks
- Manipulating a data point to make it seem like the other class (tricking the model)
- Examples:
  - Appending malicious data to raw binaries
  - Embedding goodware strings in malicious binaries
  - Creating a bias towards the detection of packers instead of truly learning the concepts of a malicious binary
- Consequences of adversarial attacks
  - Execution of malicious software
  - Poisoning ML models or drift detectors if they use unknown samples to update their definitions (no ground truth labels from other sources)
  - Concept drift and evolution

#### Direct Attacks
- White Box:
  - Adversary has full access to the model
  - Gradient-based attacks (uses weights of a neural network to create an adversarial instance that tricks the model into classifying it as another class
- Black Box:
  - Adversary only has access to the outputs produced by the model
  - Random pertubations and testing on input data as it is more challenging for adversaries since they do no know the features and classifiers used in the models

#### Posisble Solutions of Direct Attacks
- Generative adversarial networks
  - Coupled deep learning systems where one part is trained on the inputs generated by the other part
  - Generates adversaries that are used to defeat the classifier which aims to improve both parts
- Generate an algorithm that automatically generates some adversarial samples (through data augmentation or oversampling) which are used to train or update models
- Using probability labels instead of hard class labels (makes random forest-based models more resilient)
- Use only non-negative weights (Non-Negative Malconv)
  - Forces models to only look for malicious samples
  - Less prone to attackers that copy benign behavior

#### Class imbalance solutions
- As discussed in the last seminar, class imbalance occurs when the distribution of classes differs significantly
- Cost-sensitive learning (fast but hard to implement)
  - Gives each class the same importance
  - Adapt well to the learning process without generating any artificial data
- ensemble learning
  - Bagging (such as random forest)
    - Training classifiers from an ensemble with different subsets of training data
    - Diversifies the ensemble
  - Boosting (AdaBoost)
    - Trains each classifier in the ensemble using the complete training dataset in iterations
    - Gives more weight to harder samples after each iteration
    - Tries to correctly classify the incorrectly classified samples by adjusting weights 
- Anomaly detection (SVM and isolation forest) to create decision boundaries
  - Trained over the majority class and minority class samples are considered anomalous
  - The model doesn't learn the concept of minory class

#### Transfer Learning Problem
- Learning a given task by transferring knowledge from a related task that was already learned
- Has been very effective in many machine learning applications
- May be a problem as the base models are publicly available
  - Potential adversaries have access to them
  - Can produce an adversarial vector that affects both the new and base models
  - It is imperative to consider the robustness of the base model when using it to transfer learning

#### Implementation Problem
- Existing frameworks (scikit-learn and Weka) rely on batch learning algorithms
  - Not useful in dynamic scenarios where data comes in a stream
  - Require frequent updates
- Multi-language codebases can become incompatible with new releases or become to slow
  - Optimizations are not always performed as prototypes are created to simulate reality, optimizations require domain knowledge that are very specific, and implementing data stream algorithms is difficult
- Ensuring good performance for the models and algorithms is challenging

#### Possible Solutions of the Implementation Problem
- Use Scikit-Multiflow and similar frameworks for streaming data
- Adversarial ML frameworks that test and evaluate the security of ML solutions
- Optimize ML implementations with C and C++ under the hood
- Outsource model processing to third parties and scanning procedures to the cloud

#### Evaluation
- common classification metrics
  - accuracy (can provide wrong conclusions, don't use on imbalanced dataset)
  - confusion matrix
  - recall
  - precision
  - F1 score
- Comparing Apples to Oranges
  - Comparing evaluation results with other literature solutions should be carefully performed to avoid misleading conclusions
  - Leverage the same datasets when comparing with other solutions
  - Don't compare distinct approaches as they have distinct challenges
  - Researchers should share source code to enable compatibility and fair comparisons
- Delayed labels evaluation
  - There is a gap between data collection and labeling as many samples do not have the ground truth label immediately
  - Utilize or implement drift detectors that consider the delay of labels

#### Understanding ML
- An ML model is a type of signature
  - Research work claims that their approaches are better than the current signature schemes 
  - It is a pitfall as an ML model is just a weigthed signature of booleans
- There is no such thing as a 0-day detector
  - 0-day attacks leverage unknown threats and/or payloads
  - Many ML-based detectors state that their method is 0-day attack proof
  - It is a pitfall as ML models are signatures
  - Detecting "new" threats depends on the definition of "new"
  - No model will be able to detect new threats if "new" means something unknown to the model
- Security and explainable learning
  - Discussing and explaining detection results is just as important as achieving good detection rates
  - Many approaches are black box models, so it is important to explain results well for incident response
  - Explainability has various levels of relevance according to the domain
- The arms race will always exist
  - Both attack and defense sides must always invest in new approaches to overcome their adversaries
  - Defenses should reduce the gap between them and attackers

#### My Thoughts
This paper did a great job in summarizing the impact of machine learning in cybersecurity. There are pitfalls at each step of the machine learning process, so continual research and improvements is necessary to defend against adversaries. An overarching theme in cybersecurity is concept drift, and this theme will never diminish. Adversaries will always invent new ways to bypass defenses, thus it is imperative to create defenses that utilize robust drift detectors that can pick up any of the four types of concept drift and more. I found the discussion of direct attacks to be very interesting. My first thought on that was to just privatize models as that would limit attackers to black box attacks, hence increasing attack difficulty. However, I remember that open-sourcing models and approaches plays a big role in ML for security as it is a growing field that requires collaboration from all fronts. Thus, considering the proposed solutions in this paper could go a long way. Thinking like an attacker can really strengthen defenses. This can be accomplished through the coupled deep learning systems, augmented adversarial representations, or just purely accounting for malicious samples. 

The first half of the paper only discussed resampling techniques when considering class imbalance. I found the techniques from the second half to be very interesting as well. Resampling is effective, but accounting for the distribution when resampling could be more timely and less representative of reality. The proposed solutions of bagging, boosting, and anomaly detection are great alternatives as they adjust how models weight importance of each class. One thing I did not really consider when thinking of ML in security is the use of frameworks. These frameworks have their model structures predefined and do not account for the dynamic nature of security data. As the paper proposed, it can be very beneficial to outsource model processing as implementing that, on top of developing the model itself, could be timely and costly. My last big takeaway from this seminar is to be cautious when comparing approaches. Different approaches use different datasets, which in turn, produces different attributes and features. That is why open-sourcing approaches is essential as it establishes a common baseline within the community. 

---
### Discussion Summary

---
- Concept drift: malware changes (or drifts) overtime, regardless of whether you handle it or not. It's a phenomenon.
  - Can combat that by using detectors to see if the phenomenon is happening then handle the problem from there
  - Aggressive detector: constantly changes model, always have an updated model (can detect so early that a drift isnt actually happening)
  - Non-aggressive detector: detects the drift too late
  - Find a balance between the two
- Imbalanced datasets solutions
  - resampling (undersampling oversampling)
  - More powerful classifier (deep learning)
  - Adapt the classifier to learn that the weights of classes are different (cost-sensitive learning) (if you don't want to resample)
    - Adjust loss function to make model learn the weights
- Random forest is the most common (or the best in ML for cybersecurity) as it draws a non-linear decision boundary, it is the most adaptive model because of that
  - There exists ensemble of ensemble (random forest with SVM where sometimes one model classifies, another time another model classifies, or there is a majority vote for which model to use)
  - Ex: random forest on the user device and neural network on the cloud (since user device is going to be updated more frequently)
- Hierarchical clustering: rather than having many classes (like a million malware families) you cluster a subset based on families that represent the million well
- A way to improve performance and works for outlier detection is one-class classifier: is this data point in this class or is it one of the other million classes? We don't care if it is a part of the others.
- 0-day attacks threats that are unknown to detectors

#### My Thoughts
This discussion helped to expand upon points discussed in the seminar. As with anything, there are different drift detection procedures. We discussed aggressive and non-aggressive detectors. Both have their pros and cons. Aggressive detectors ensure that models are up to date with the newest features, but can result in false positives where they detect a drift when there actually isn't a drift occurring. Non-aggressive detectors, on the other hand, combat that issue but can also potentially miss the occurrence of a drift. A good solution would be to find a hybrid between these two. As presented in the seminar, imbalanced datasets can be fixed through resampling, cost-sensitive learning, bagging, boosting, and anomaly detection. However, how do these methods actually solve the issue of imbalanced datasets? Regarding the latter three, they adjust the weights of specific classes, specifically through the loss function. In any machine learning model, the loss function minimizes the error by adjusting the weights of the model (either through decreasing the importance less important features or vice versa). 

In this discussion, our class concluded that random forest is the best for many cases in ML for cybersecurity as it draws a non-linear decision boundary. Classic classification techniques such as logistic regression or SVM are simple, but only draw linear decision boundaries. Thus, these models do not accurately represent the feature difference between malicious and benign classes. It is easy to understand now why random forest performs the best, but as time surpasses, there could be other models or variations of random forest that even more accurately draw boundaries between classes. The idea of one-class classifiers is very intriguing. As mentioned before, samples can be associated with one of millions of classes. It is inefficient to draw decision boundaries for all classes in that case. Therefore, one-class classifiers can be used to increase efficiency by pinpointinh if a sample labeled as one class, and if not continue finding the samples associated with this class. Lastly, I agree with the statement that there are no such things as 0-day detectors. No ML model can detect if a new, unseen sample is malicious or not, as it has never seen that sample in its history. ML models must update their feature representations and weights through retraining before even considering that new sample.

---
That is all for this seminar, thanks for reading!
