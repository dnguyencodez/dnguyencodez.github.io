## Dos and Don'ts of Machine Learning in Computer Security

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic

During this seminar, Nhat Nguyen and Sonjoy Kumar Paul presented section 2 of [Dos and Don'ts of Machine Learning in Computer Security](https://www.usenix.org/system/files/sec22summer_arp.pdf). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information discussed by Nhat and Sonjoy as well as a summary of our class discussion.

### Presentation Summary

---
#### ML Workflow and Common Pitfalls
- The data collection and labeling phase is typically accompanied by sampling bias and label inaccuracy
- The system design and learning phase is typically accompanied by data snooping, spurious correlations, and biased parameters
- The performance evaluation phase is typically accompanied by inappropriate baseline and measures, and base rate fallacy
- The deployment and operation phase is typically accompanied by lab-only evaluation and inappropriate threat model

#### Data Collection and Labeling
- Sampling bias:
  - Occurs when the collected data does not sufficiently represent the true data distribution of the underlying security problem
  - Highly relevant to security as data collection is a challenging task
  - A real world example of sampling bias is WWII data of aircraft bullet damage on planes denoting survival (survivorship bias)
    - If the aircraft was reinforced in the most commonly hit areas, essential data was being ignore (the planes that were hit in other areas did not survive)
- Recommendations to prevent sampling bias:
  - Construct various estimates of the true distribution and analyze them individually
  - Extending the solution with synthetic data
  - Transfer learning
  - Mixing data (occasionally)
- Label inaccuracy:
  - The ground-truth labels required for classification tasks are inaccurate, unstable, or erroneous, affecting the overall performance of a learning based system
  - In security, reliable labels are often not available resulting in researchers using heuristics through external tools, the inability to adapt to the dynamic nature of adversarial behavior, and performance decay when deployed
- Recommendations to prevent label inaccuracy:
  - Verfiying labels (manually investigating false positives or random samples)
  - Handling noisy labels by using robust models, consistently modeling label noise in training, or data cleansing
  - Preventing label shift by delaying labeling until a stable ground-truth is available
  - A case study demonstrated the effects of noisy labels
    - It was shown that noisy labels caused a significant performance drop (F1-score decreasing to 0.73 from 0.95)
    - Data cleansing brought the F-score back to 0.93
    - Only 0.2% of clean labels falsely flagged as noise

#### System design and learning:
- Data snooping:
  - A learning model is trained with data that is typically not available in practice. Data snooping can occur in many ways, some of which are very subtle and hard to identify
  - Test snooping occurs when the test set is used for experiments before final evaluation
  - Temporal snooping occurs when time dependencies are ignored even though distributions are under continuous change
  - Selective snooping describes the cleansing of data based on information not available in practice
  - Data snooping can lead to overoptimistic results
- Recommendations to prevent data snooping:
  - Isolating the test data until final evaluation
  - Consider temporal dependencies when splitting the data
  - The use of well known datasets should be complemented with more recent datasets
- Spurious correlation:
  - Artifacts unrelated to the security problem create shortcut patterns for separating classes. Consequently, the learning model adapts to these artifacts instead of solving the actual task.
  - Artifacts which correlate with the task to solve but do not actually relate to it
  - Example: a network instrusion system with the majority of the attacks in the dataset come from a specific network region. The model will learn to detect a certain IP range instead of generic attack patterns
  - Spurious correlations are often unidentified because ML is typically a black box in security
  - This can lead to overestimating the capabilities of an approach making it impractical in practice
- Recommendations to prevent spurious correlations:
  - Make the models explainable, providing full analysis which can reveal spurious correlations and allows individuals to asses their impact
  - Clearly define the objective in advance and fully validate if spurious correlations from one setting are deemed valid in another
- Biased parameter selection:
  - The final parameters of a learning-based method are not entirely fixed at training time. Instead, they indirectly depend on the test set
  - Models can perform very differently if their parameters are not fully set during training
  - Real-world traffic is much more diverse
  - Related to data snooping
- Recommendations to prevent biased parameter selection:
  - Use similar countermeasures as data snooping
  - Use a separate validation set for model selection and parameter tuning
  
#### Performance Evaluation
- Inappropriate baseline:
  - The evaluation is conducted without, or with limited, baseline methods. As a result, it is impossible to demonstrate improvements against the state of the art and other security mechanisms
  - It is vital to compare with previously proposed methods
  - There is no such thing as universal approaches that outperform every other
- Recommendations to prevent the effects inappropriate baselines:
  - Consider simple models as well as complex models
  - Simple models are easier to explain and have been proven to be effective
  - Automated ML can help find proper baselines
  - Check whether non-learning approaches are also suitable
- Inappropriate performance measures:
  - The chosen performance measures do not account for the constraints of the application scenario, such as imbalanced data or the need to keep a low false-positive rate
  - Examples: using only accuracy, showing only ROC curves, not accounting for multi-class metrics (since many security issues deal with more than two classes)
  - True and false positives are typically good at providing performance measures, but they can also disguise the actual precision when the prevalence of attacks is low
- Recommendations to prevent the effects of inappropriate performance measures:
  - Choice of performance measure in ML depends on the application
  - Thus, consider the practical deployment and observe from there
- Base rate fallacy:
  - A large class imbalance is ignored when interpreting the performance measures leading to an overestimation of performance
  - Relevant in many security problems like intrusion detection and website fingerprinting
  - Probability of installing malware is typically much lower than is considered in experiments
- Recommendations to prevent the effects base rate fallacy:
  - Use precision and recall in applications that detect rare events as they account for class imbalance and encompass reliable performance indicators for detectors focusing on a minority class
  - Consider Matthews Correlation Coefficient (MCC) when the prevalence of the minority class is inflated
  - Discuss false positives regarding the base rate of the negative class

#### Deployment and Operation
- Lab-only evaluation
  - A learning-based system is solely evaluated in a laboratory setting, without discussing its practical limitations
  - Evaluating in controlled environments is legitamite, but evaluating in realistic settings better assesses capability and any challenges for further research
  - Detection methods evaluated only in closed-world settings with little to no diversity and no consideration of non-stationarity
    - Many website fingerprinting attacks are evaluated only in closed-world settings over a short time period
- Recommendations to prevent the effects of lab-only evaluation:
  - Approximate real-world settings as accurately as possible
    - Consider temporal and spatial relations of data to account for reality
  - Runtime and storage constraints need to be analyzed under practical conditions
  - The proposed system should be deployed to identify problems not found in lab experiments
  - Inappropriate threat model:
  - The security of machine learning is not considered, exposing the system to a variety of attacks, such as poisoning and evasion attacks
  - ML algorithms are vulnerable to various attacks like adversarial preprocessing, poisoning, and evasion
  - Adversarial attacks can lead to untrustworthy output and meaningless results
  - Easier to evade a model that relies on few features as opposed to a model that is properly regularized and developed with security in mind
  - Semantic gaps (such as imprecise parsing and feature extraction) can further enable malicious activity
- Recommendations to prevent the effects of inappropriate threat models:
  - Models and systems should account for adversarial ML (assume an adaptive adversary in most cases)
  - Consider all stages of the ML workflow and investigate any vulnerabilities
  - Evaluation of adversarial aspects is mandatory

#### My Thoughts
