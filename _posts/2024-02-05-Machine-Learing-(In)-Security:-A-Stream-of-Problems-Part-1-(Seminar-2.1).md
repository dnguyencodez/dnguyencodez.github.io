## Machine Learning (In) Security: A Stream of Problems Part 1 (Seminar 2.1)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

This seminar was presented by me! I covered sections 3, 4, and 5 of [Machine Learning (In) Security: A Stream of Problems](https://arxiv.org/abs/2010.16045). After the presentation, our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary
This seminar covers the first part of pitfalls in standard machine learning practices when applied to cybersecurity, and how to overcome them. Below are the main ideas presented.

#### Background of Machine Learning in Cybersecurity
- There are various types of security tasks such as attack detection, incident response, security analysis, and system forensics
- Each task can be associated with more specialized attacks such as malware detection, spam filtering, log analysis, object recognition, and etc.
- These tasks can be automated and remedied through machine learning tasks such as classification, clustering, reinforcement learning, and dimensionality reduction
- Different tasks can be solved using similar machine learning methods
  - For example, groups of forensics being clustered and groups of logs being clustered
- Machine learning traditionally consists of a stream of data being collected and processed, features and attributes being extracted, using the preprocessed data to train a model, evaluating the model against metrics, updating necessary weights and parameters, and repeating that process until convergence
- This presentation analyzes the pitfalls and solutions of the first half of the machine learning process (data collection, attribute extraction, and feature extraction)

#### Data Collection
- Collecting representative data is essential in building effective models for security tasks, thus it can be one of the most challenging parts
- Generally, data is available in three formats
  - **Raw data**: data that is in its original form which could be used to analyze the original behavior (portable executable binaries, APK packages, network traffic data captured, etc.)
  - **Attributes**: data that is filtered from raw data with less noise, allowing for more focus on important data (CSV with metadata, execution logs of software, etc.)
  - **Features**: Features extracted from attributes or raw data that distinguishes samples (transformation of logs into feature vectors, transformation of traffic data into features containg attribute frequency, etc.)
- A common issue in ML systems for security is **data leakage**
  - Common practice for many machine learning practices is k-fold cross validation which splits the dataset into ***k*** partitions and uses one of the partitions as the test set and the others as training sets for evaluating a model ***k*** times
  - This is not practical for cybersecurity data as using mixed epochs to train a model does not help in determining future threats since data is obtained from a stream (this is known as temporal inconsistency)
  - An example is a malware detector operating similarly to an antivirus
    - It is trained on past malicious and benign software
    - The model then detects if future files are malign and updates the model when knew attacks are discovered, increasing the coverage of unknown attacks
    - Timeline matters in malware detection!
  - It is so important to collect timestamps of samples during data collection as it can combat the issue of data leakage
- **Data labeling** involves labeling collected artifacts which is essential for training and updating models
  - It is as difficult as collecting the artifacts in the first place
  - Many cases consist of making hypotheses of crawled artifacts
    - Labeling all samples collected by crawling a blacklist as malicious and all samples from an App Store as benign
    - Not very reliable as even samples from known sources could be trojanized and/or inaccurate, leading to models learning some malware as legitimate
  - Using labels assigned by AVs (antiviruses) can mitigate those problems
    - AV labels vary, so using a specific AV's labels or finding a way to unify all AV labels is important
    - AV's provide two labels: family attribution (the family of malware) and detection (information regarding the artifact)
    - The two labels change over time where new samples are assigned a generic or estimated label initially and updated as more knowledge of the sample is obtained
    - Thus, the labeling date of samples can severely impact the model's results
    - Consider using delayed evaluation to account for these issues, which is discussed further in the paper
- In the data collection and processing phase, **class imbalance** is another issue commonly seen in datasets
  - The data distribution between the classes of a dataset differ by a large margin, which makes malware detection more challenging
  - The solution discussed in this presentation is resampling (undersampling and oversampling)
  - Undersampling removes instances from the majority class
    - 
