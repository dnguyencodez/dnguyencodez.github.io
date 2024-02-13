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
    - 
