## Automatic Yara Rule Generation Using Biclustering (Seminar 12.1)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Yash Shende and Pratik Nath presented [Passphrase and keystroke dynamics authentication: Usable security](https://arxiv.org/pdf/2009.03779.pdf). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Introduction
- Machine Learning is increasingly used in malware detection, but traditional signature-based methods like Yara, which uses content matching and logical rules to identify malware, remain crucial
- Developing effective Yara rules is time-consuming, especially for junior analysts, and there's a growing backlog of samples needing analysis, with few attempts at automating Yara rule development
- The paper proposes AutoYara, a tool to automate Yara rule development, aiming to save analysts time by generating rules with low false positives, not to replace human analysts but to assist in managing workloads
- AutoYara employs a two-step process involving finding candidate byte strings and extending the SpectralCoClustering algorithm for building complex logic rules, showing effectiveness in reducing the time analysts spend on rule construction

#### Related Work
- Yara is a key tool for malware analysis, supported by other tools like YarGen and VxSig, which are used to generate Yara-compatible signatures, but other malware classification systems that don't produce Yara rules are not considered alternatives
- YarGen uses a Naive Bayes model and heuristics to select and combine features from binaries for rule construction, but often requires manual refinement to produce useful rules
- VxSig, another tool, employs a least-common-subsequence algorithm to identify common byte sequences in malware samples but needs human intervention and significant processing time
- Despite various attempts at automated Yara rule generation, including "Greedy" methods and the use of "KiloGrams" for computational efficiency, these approaches face limitations in effectiveness and practicality, underscoring the challenges and inherent weaknesses in signature-based malware detection

#### Improved Spectral Coclustering
- Biclustering algorithms are used to simultaneously cluster rows and columns of a matrix, revealing structures that inform the construction of Yara rule signatures
- The approach facilitates the identification of feature combinations for Yara rules, using “and” statements to reduce false positives and “or” statements to increase true positives, forming a "disjunction of conjunctions" rule structure
- Challenges include determining the unknown number of biclusters without enforcing non-overlapping constraints, desired for flexibility and comprehensive rule generation
- The methodology extends SpectralCoClustering with Variational Gaussian Mixture Models (VGMM) to automatically determine the number of biclusters, allowing for overlap and excluding irrelevant rows/columns
- The process involves a detailed algorithm that improves on traditional methods by accommodating overlapping biclusters and automatically adjusting to the data's complexity and size, specifically tailored for malware analysis scenarios

#### AutoYara Design
- AutoYara is designed to be lightweight for use on low-resource machines, aiming for minimal memory use and no GPU reliance, producing Yara rules within minutes for efficiency and interpretability by analysts
- It builds Yara rules based on specific byte patterns using large byte n-grams (8-1024) and utilizes an algorithm for extracting frequent n-grams efficiently, ensuring they are interpretable by malware analysts
- To ensure specificity, it filters out common n-grams using a Bloom Filter and applies additional criteria, such as entropy measures and byte value checks, to remove unsuitable n-grams
- The tool's algorithm, AutoYara, creates Yara rules by extracting top-k most frequent n-grams, filtering them, and then clustering them into rules through a sophisticated method that balances feature frequency and specificity
- The final implementation is in Java for cross-platform compatibility, aiming for fast execution and low false positive rates by penalizing rules with fewer n-gram features, and it's available on GitHub with the JSAT library for biclustering

#### Datasets
- The AutoYara system is trained on the Ember 2017 corpus, using 600,000 files to build Bloom Filters for identifying malicious files, without using the test set to ensure rigorous evaluation
- A larger dataset from VirusShare, labeled with the help of AVClass tool, is used to evaluate AutoYara's accuracy on malware families, aiming for a low false positive rate by testing against a varied collection of malware and benign files
- AutoYara's effectiveness is demonstrated in a practical scenario by significantly reducing the time required for analysts to write Yara rules, showcasing its utility in real-world applications and its potential to improve efficiency in malware identification tasks

#### Experimental Validation
- Large Scale Testing
  - The experiment evaluated the effectiveness of Yara rule generation methods across a broad range of malware families and sample sizes, finding Greedy and VxSig methods inadequate due to low rule viability and high computational demand, respectively
  - AutoYara outperformed YarGen in generating higher-quality rules across all tested sample sizes, with YarGen's rules not being practically usable until 128 samples were provided, indicating AutoYara's superiority in rule quality and efficiency
  - YarGen showed poor performance due to a bias that decreased true positive rates as false positive rates increased, whereas AutoYara demonstrated the ability to generate rules with low false positives and higher true positive rates across a range of sample sizes, leading to its exclusive consideration for further analysis
- Retro hunt results
  - Analysts deploy to unfamiliar networks to search for malware, possibly without knowing the specific type. AutoYara helps by creating rules to find similar malware, with any malware discovery being relevant
  - Using VirusTotal's Retro Hunt and malware samples, AutoYara's effectiveness in generating Yara rules was tested, revealing low false-positive rates and high true positive (TP) rates for several malware families
  - AutoYara demonstrated a 100% TP rate for 11 out of 20 malware families, outperforming VxSig in 15 of the 20 cases, across over 90 million files
  - While most results were encouraging, some failures occurred with specific malware, highlighting the technology's limitations and the high FP rate in certain cases
- Industry Human Comparison Testing
  - Professional malware analysts were tasked with developing Yara rules for detecting malware, aiming for high coverage with minimal false positives; challenges included time constraints and incomplete tasks due to new priorities
  - AutoYara significantly outperformed traditional methods in creating useful detection rules for 21 out of 24 malware families, with low false positive rates and saving considerable analyst time
  - Analysts' manual efforts often fell short due to time constraints and high false positive rates, while AutoYara provided a viable alternative with its ability to generate efficient rules quickly
  - Compared to previous methods like Greedy and VxSig, AutoYara showed superior performance in rule generation, also allowing easy modifications by users with minimal experience, further enhancing its utility

#### When Will AutoYara Work Well?
- Byte similarity investigation
  - AutoYara's effectiveness in malware detection is linked to the similarity of malware samples, assessed using SSDEEP scores, with scores ≥20 indicating a match
  - Visualization of malware families as densely connected subgraphs based on SSDEEP scores reveals natural clustering, aiding AutoYara's performance through the creation of efficient rules
  - The variation in SSDEEP scores within different malware families, like Xpaj and Zlob, highlights the challenges in generating effective rules due to the presence of disparate and noisy data
- Reverse engineering investigation
  - Reverse engineering of AutoYara-generated rules for malware families showed targeting of main file sections, with specifics varying by section
  - Both AutoYara and a human analyst targeted similar file sections (.text, .rdata, .data, .rsrc, .reloc) but showed preferences for different sections, with AutoYara favoring the .text section for its high entropy
  - There is a high correlation between AutoYara and manually created rules in targeting file sections, indicating similar strategies in identifying malicious content
  - Analysis of rule effectiveness revealed a strong overlap in the executables flagged by both AutoYara and manual rules, despite some differences in false-positive rates

---
### Discussion Summary

---
- These authors are proposing an approach that utilizes machine learning and rules which make it more readable
- They are using biclustering to cluster in two dimensions instead of one (two constraints)
  - Ex: try to maximize the coverage in one direction and minimze the coverage in the other which combats issue with single clustering
- AVs use both ML and signatures
  - Many people make bold statements that AVs use only one, but they are wrong
  - Sometimes ML works better for some cases whereas signatures work better with others
  - Concept drift is a good example of use case
    - When a new malware sample is encountered, it takes a lot of effort and time to retrain
    - Thus, in the meantime, create and extract the signature
  - Signatures and using Yara rules are much faster than ML models since Yara rules are byte based
- Another pitfall is that if an AV uses a rule then no ML is used
  - The rules can be generated by an ML model
- Storing the rules is complicated so what is an efficient way to store the rules?
  - Bloom filter: probablistic data structure that reduces the storage size, however a tradeoff is that it increases false positives
    - Data is passed through hash function and then stored as an index in the array
    - A specific byte will always be mapped to an index/value
    - Drawback is that bloom filters use a hash function which means potential for collisions, and hence some bytes will be mapped to different and irrelevant indicies/values
    - The drawback only increases FPs, not FNs
  - Bloom filters are typically a good solution and should be used more
 
---
That is all, thanks for reading!
