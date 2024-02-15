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
