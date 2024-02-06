## Machine Learning (In) Security: A Stream of Problems Part 1 (Seminar 2.1)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

This seminar was presented by me! I covered sections 3, 4, and 5 of [Machine Learning (In) Security: A Stream of Problems](https://arxiv.org/abs/2010.16045). After the presentation, our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
This seminar covers the first part of pitfalls in standard machine learning practices when applied to cybersecurity, and how to overcome them. The presentation analyzes the pitfalls and solutions of the first half of the machine learning process (data collection, attribute extraction, and feature extraction). Below are the main ideas presented.

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
  - **Undersampling** removes instances from the majority class
    - Can affect the data representation as it may reduce the detection of some malware families
    - Lowers the importance of sample importance compared to reality within a given time period
    - An example in this paper is presented where there are two classes
      - The results show that undersampling with temporal information and keeping the same proportion of sub-clases of a given class contains the same, original distribution
  - **Oversampling** is the opposite, where synthetic instances are created for the minority class
    - One technique is SMOTE which selects samples that are similar in the feature space, drawing a line between them, and generating new samples following that line
    - Using a similar experiment as undersampling, the authors described how oversampling can also result in data leakage if time is not considered
      - Oversampling utilizing temporal information results in the same proportioned distribtution of class data as the original dataset
    - Oversampling can only generate feature vectors, not synthetic raw data
  - The last component of the data collection process with pitfalls discussed is **dataset size definition**
    - Small datasets are not truly representative of a real scenario and make it hard for a model to generalize
    - It is hard to collect and evaluate real artifacts as many organizations do not share their vulnerabilities
    - Large datasets, on the other hand, take much longer to train on and can result in complex models and decision boundaries that are not feasible in reality
    - In this paper it was shown that reducing dataset complexity results in higher accuracy but is not truly representative of reality
    - Another graph shows that models have bias towards the scenario (or region) where the dataset used to train them was collected
    - This paper presents the results from two experiments
      - The first experiment tested several classifiers (multi-layer perceptron, linear SVC, random forest, and naive bayes)
        - AndroZoo dataset was used with it being divided by months
        - The results demonstrate that all classifiers had similar behaviors and reached optimal classification using 10-20 percent of the original dataset
      - The second experiment used random forest in four different approaches
        - The four approaches are cross-validation, temporal classification, stream with no drift detector, and stream with ADWIN drift detector
        - Similar to the first experiment, all approaches presented the same behavior with optimality being reached at 30-40 percent of the original dataset

#### Attribute Extraction
- It is extremely important for creating features in machine learning models
- Understanding the difference between attributes and features are important
  - Attributes are filtered metadata from raw data and only pertains to the security task being performed
  - Features are attributes transformed into a distinct set of samples representation that are ready to be inputted into a model
- Different attribute extraction procedures result in different outcomes
- There are two kinds of attributes: static and dynamic
  - Static attributes are the characteristics of a file that can be examined without executing the file (such as the presence of a fork system call in a sample's binary)
  - Dynamic attributes are the characteristics and behaviors observed when malware is executed in a controlled environment such as a sandbox (frequency of the fork system call invocation in a sandbox)
- An experiment was performed with attribute extraction procedures (static and dynamic) over three different pairs of models to detect linux malware
  - The three models are SVM with RBF, multi-layer perceptron, and random forest
  - The results show that, for each pair, dynamic attributes produced higher accuracy, with random forest having a very small margin

#### Feature Extraction
- The last step in the data collection phase is feature extraction
- Concept drift describes the evolution of attacks and malware as time progresses, urging defenses to account for this advancement
- Feature extractors must constantly be updated due to the changing nature of malware
  - In traditional data pipelines feature extraction is applied to the raw data stream before the cycle of updating the model
  - The proposed data pipeline updates the feature extractor within the process of updating the model to mitigate the problem of concept drift
- An experiment is discussed using the AndroZoo and DREBIN datasets
  - Samples are sorted by their first seen date
  - Two Adaptive Random Forest classifiers are trained and both include the ADWIN drift detector
  - Both classifiers are trained using the traditional and proposed data pipelines with their results compared
  - The results demonstrate that the proposed data pipeline (updating the feature extractor) performs better overall
- Along with evolving malware, attackers may try to adapt their malicious features to be similar to benign ones, ultimately tricking defenses
- This is known as **adversarial machine learning**
  - This calls for defenses to accomodate for the thought process of attackers
  - The experiments in this paper discusses two different kinds of feature representations
    - The first kind is MalConv where raw bytes of an input file represent features
    - The second kind is LightGBM where feature matrices are constructed using a hashing trick and histograms based on binary file input characteristics
    - The raw bytes feature extraction can be tricked by adding goodware bytes at the end of malicious binaries (does not affect original binary execution and biases the malware detector towards these goodware bytes)
    - The feature extraction technique using file characteristics can be tricked by embedding the original binary into a file dropper which extracts malicious content and executes it
    - The external dropper only contains file characteristics of benign samples, allowing the malware to sneak through
- Thus it is important to update the feature extractor and make robust features!

#### My Thoughts
This seminar really helped me in discovering distinctions between standard machine learning and machine learning for cybersecurity. For instance, I always believed that class imbalance could be solved by changing the performance metrics or through utilizing ensemble methods. Resampling has always been an option as well, but this paper helped me to discuss the issues that accompany each resampling technique. Ensuring that datasets keep their original distribution is essential as that is what makes them representative of reality. Additionally, imbalanced classes can have a more adverse affect on models in cybersecurity compared to traditional ML models as the goal is to minimize the false-positive rate. Imbalanced classes in malware detection and utilizing resampling without incorporating consistent distribution can indadvertently change FPR for the worse. For instance, oversampling on a dataset with malicious samples being the minority class can cause the model to predict more files as malicious when they should be benign. Another important factor of ML in cybersecurity that I did not realize is temporal information. Not only does it significantly help the class imbalance problem, but timestamps are essential in mimicking practical scenarios where data is being inputted in a stream. Models could not be trusted if they don't account for the time and position of a sample.

Attribute and feature extraction play essential roles in the machine learning process. These components are what makes up a dataset and explain to a model why each sample is labeled the way it is. Binaries are different than standard ML data formats, so it is imperative that the most contributing attributes are extracted. I believe that when performing the attribute extraction procedure, it should be done manually for various samples so that the engineers behind it can actually see what attributes actually make a file malicious. Then, it can be automated thereafter. Additionally, the paper's results show that dynamic attributes generally help a model to perform better, and I agree. Dynamic attributes better represent real-world scenarios as they actually encompass the execution of samples. A general rule in ML is to extract robust features and that does not change in cybersecurity. 

--- 

### Discussion Summary

---

As I was primarily focused on pulling up real-world data samples that I missed, I was not able to write many discussion notes down. However, I below are the points I was able to obtain/remember.
- Data labels constantly change
  - When a new sample appears, it is initially labeled as generic or benign
  - As more evaluation completes, that label is updated
- Typically, we do not need all of a dataset
  - Performance plateaus after a proportion of the data is used
  - Another way to combat a plateau in performance is to use a completely different dataset or features

#### My Thoughts
These discussion points that I remember expand on the information discussed in the paper. As the paper mentioned, labeling data is an important part of data collection pertaining to malware. Many AVs update the labels as time passes. We were able to see an example of this through VirusTotal which displays the labels assigned to a specific file from various security vendors. The file we looked at was labeled as gen (generic) by one vendor whereas it was labeled as a TrojanBanker from another vendor. This example helped reiterate the point in the paper that considering time in the labeling process can really aid a model's performance in deployment.

Additionally, both the paper and our discussion covered how all samples in a dataset are not always needed. A portion of a dataset can be used as the classification perfomance plateaus eventually. It is important to keep the distribution in mind, as reiterated throughout the paper. Another way to look at performance convergence is how the model is learning. If a model's performance converges only using 20% of the data, does that mean that it's perfect? Not necessarily. It could mean that the model is only learning the data and features within the dataset being used. Thus, another way to experiment with performance boost is to train on a completely new dataset with new features.

---
That is all for this seminar, thanks for reading!
