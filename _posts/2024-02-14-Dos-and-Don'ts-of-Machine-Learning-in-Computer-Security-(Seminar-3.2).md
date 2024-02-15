## Dos and Don'ts of Machine Learning in Computer Security (Seminar 3.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic

During this seminar, Sidharth Arivarasan and Sahil Salunkhe presented sections 3 and 4 of [Dos and Don'ts of Machine Learning in Computer Security](https://www.usenix.org/system/files/sec22summer_arp.pdf). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Review
This paper covers pitfalls of machine learning in computer security along with potential solutions. Last seminar discussed pitfalls regarding data collection and labeling, system design and learning, performance evaluation, and deployment and operation.

#### Prevalence Analysis
- It is important to assess the prevalence and investigate the impact of pitfalls on scientific advances
- A study was conducted on 30 papers published at the top-4 conferences for security research (ACM CCS, IEEE S&P, USENIX Security, NDSS)
- Topics covered: Malware Detection, Network Intrusion Detection, Vulnerability Discovery, Website Fingerprinting Attacks, Social Network Abuse, Binary Code Analysis, Code Attribution, Steganography, Online Scams, Game Bots, Ad Blocking
- Review process:
  - Thorough assessment of each paper by two independent reviewers from a pool of six researchers experienced in machine learning and security
  - After individual assessments, reviewers discuss their findings together, involving a third reviewer to resolve disputes
  - The review process is iterative, ensuring that the pitfalls identified accurately represent core issues in the field
  - The process has been validated through high inter-rater reliability (Krippendorff’s alpha = 0.832), indicating that the ratings are confidently reliable
- Assessment criteria:
  - Pitfalls are classified into four categories: present, not present, unclear from text, or does not apply, based on their occurrence and handling within the experiments
  - A pitfall is considered "not present" if it has been fully addressed or mitigated by the authors, or if the scope of claims has been appropriately narrowed to avoid the pitfall
  - "Partly present" is used for experiments affected by a pitfall, yet the impact has been somewhat mitigated
  - If it's not possible to determine the presence of a pitfall due to insufficient information, the review marks the pitfall as "unclear from text"
- Observations:
  - Sampling bias and data snooping are the most common pitfalls, present in 90% and 73% of papers respectively
  - Over half the papers display issues with inappropriate threat models, lab-only evaluations, and inappropriate performance measures
  - Every analyzed paper has at least three pitfalls, highlighting widespread problems within recent computer security research
  - The clarity of experimental procedures is a notable issue, with pitfalls like biased parameter selection, spurious correlations, and data snooping often being unclear from the text, affecting the reproducibility of experiments
- Feedback:
  - A survey asked authors if they found the researchers' work helpful and collected their opinions on the presence and avoidability of specific pitfalls in their papers, using a five-point Likert scale for responses
  - 49 authors responded (36% response rate), representing 43% of the researched publications; 95% had read the paper, and 98% agreed it helps raise awareness of the pitfalls
  - Authors on average agree with 63% of the identified pitfalls in their work
  - Authors believe that some pitfalls, such as inappropriate performance measures and parameter selection, are easier to avoid than others, indicating a need for clearer definitions and guidelines in the community
- The main takeaway is that there are many pitfalls in ML for security but the gaol is not to criticize researchers, rather to raise awareness of these pitfalls

#### Impact Analysis
- The authors estimate the experimental impact of a few of the pitfalls in applications of ML in security
- Effects of sampling bias, spurious correlations, and inappropriate performance measures on Android malware detection:
  - Data collection:
    - AndroZoo dataset is used (more than 11 million Android applications from 18 sources)
    - Demonstrates that experiments suffer from severe sampling bias
  - Dataset analysis:
    - AndroZoo is distributed based on app origin and antivirus detections (GooglePlay, Chinese markets, VirusShare, others)
    - The probability of sampling an app from GooglePlay is around 80% with no antivirus detection constraints, while apps with over 10 detections are 70% likely to come from Chinese markets, suggesting sampling bias
  - Experimental setup:
    - Two datasets are used for experiments: D1 combines 10,000 benign apps from GooglePlay with 1,000 malicious apps from Chinese markets, while D2 uses the same benign apps but with 1,000 malicious ones from GooglePlay
    - All malicious apps included are detected by at least 10 antivirus scanners to ensure their malicious nature
    - A linear support vector machine is trained on both datasets, utilizing feature sets from established classifiers, DREBIN and OPSEQS
  - Results:
    - Recall rates for DREBIN and OPSEQS decrease significantly by over 10% and 15% when comparing datasets D1 and D2, hence the type of performance metric used is critical
    - The classifiers appear to distinguish Android apps based on their origin, with the URL "play.google.com" being a top feature for benign apps
- Vulnerability discovery
  - Vulnerabilities in source code can lead to major threats like privilege escalation and remote code execution (raises the importance of preprocessing and classification using artifacts)
  - Dataset collection:
    - Source code from the National Vulnerability Database and the SARD project
    - Focus related to buffers (CWE-119) obtaining 39,757 source code with 10,444 (26%) labeled as containing a vulnerability
  - Dataset analysis:
    - Random code snippets were analyzed by hand to spot possible artifacts
    - Buffer size of char arrays are considered for investigation
    - A model's reliance on buffer sizes as a feature indicates spurious correlation
  - Experimental setup:
    - VulDeePecker trained based on a recurrent neural network to classify code snippets automatically and another SVM with bag-of-words as a baseline.
    - Variable names were replaced with generic identifiers and were shortened to 50 tokens per snippet
    - Layerwise Relevance Propagation was used for predictions and to assign a relevance score to each token denoting the importance
  - Results:
    - The most occurring tokens from the top-10 important tokens are extracted and reported in descending order of occurrence
    - ‘(’, ’]’, ‘,’ are among the most important features even though they occur frequently in code as part of function calls or array initialization
    - VulDeePecker heavily relies on common code tokens and artifacts, suffering from spurious correlations
    - Simple SVM using 3-grams outperformed VuldeePecker
    - Truncating the snippets to a fixed length loses important information
  - Source code author attribution
    - Identifying the developer based on source code
    - Programming patterns characterize habits so that methods use an expressive set of features
    - Dataset collection:
      - The study uses data from the Google Code Jam (GCJ) programming competition, where participants solve coding challenges
      - The dataset includes 1632 C++ files from 204 authors solving eight challenges in the 2017 GCJ
    - Dataset analysis:
      - Developers often reuse code templates across challenges, introducing a bias in the dataset
      - This copying is common due to the competition's nature, where speed matters
      - Current attribution methods may have issues due to expressive features and spurious correlations, leading to overestimated accuracy
    - Experimental setup:
      - Evaluates the impact of coding artifacts on machine learning models using state-of-the-art attribution methods and a custom linter tool based on Clang
      - Three experiments are conducted: using an unprocessed dataset as a baseline, testing on a dataset with unused code removed, and retraining the classifier on a dataset with unused code removed
      - The goal is to determine the extent to which machine learning classifiers rely on irrelevant template code in vulnerability detection
    - Results:
      - If we remove unused code from the test set (T1), the accuracy drops by 48 % for the two approaches. This shows both systems focus considerably on the unused template code
      - After retraining (T2), the average accuracy drops by 6 % and 7 % for the methods of Abuhamad et al. and Caliskan et al
  - Network intrusion
    - Detection of anomalous network traffic relies heavily on learning-based approaches
    - Generating synthetic data is often insufficient for justifying the use of complex models
    - Dataset collections:
      - Capture of Internet of Things network traffic simulating the initial activation and propagation of the Mirai botnet malware
    - Dataset analysis:
      - Analyze the transmission volume divided into bins of 10 seconds
      - A strong signal in the packet frequency indicates an ongoing attack
      - Could result in spurious correlations
    - Experimental setup:
      - KITSUNE is used which is built on an ensemble of autoencoders
      - Boxplot Method with packets using a 10-second sliding window
      - Lower and Upper threshold are derived from the clean calibration distribution
      - Packets are marked benign if the window's packet frequency is between low and high
    - Results:
      - Along with its superior performance, the boxplot method is exceedingly lightweight compared to the feature extraction and test procedures of the ensemble
      - With no appropriate baselin you can't justify complexity and overhead
      - Simple methods also result in issues
    
