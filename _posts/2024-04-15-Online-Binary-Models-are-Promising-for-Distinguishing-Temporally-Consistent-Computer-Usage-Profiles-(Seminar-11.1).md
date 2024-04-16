## Online Binary Models are Promising for Distinguishing Temporally Consistent Computer Usage Profiles (Seminar 11.1)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Erik Muller presented [Online Binary Models are Promising for Distinguishing Temporally Consistent Computer Usage Profiles](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9786768). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Introduction
- Computer User Profiling: A method to create digital identities based on individual computer usage patterns, including network traffic and keyboard dynamics, which can distinguish users by their activity and consistency
- Usage and Temporal Consistency: Profiles can uniquely identify users through their specific computer usage habits and the regularity of these activities over time
- Application in Continuous Authentication (CA): Computer usage profiles can improve CA by being automatically recorded in a non-intrusive manner, despite potential privacy concerns
- Research Focus: The study explores the consistency, uniqueness, and key features of computer usage profiles over time without proposing a specific CA solution, aiming to inform the design of robust CA methods
- Findings: Profiles generally maintain consistency, can uniquely identify users, with network features being crucial for differentiation, and online binary models performing best in profile distinction and adapting to usage changes

#### Threat Model and Assumption
- CA methods are recommended for corporate environments to mitigate risks from both internal and external threats, despite not proposing a new CA solution, with an acknowledgment of employees' low privacy expectations
- The development of CA systems aims to record computer usage profiles to enhance security, with a caution against creating privacy-invasive solutions without informing users of their limited privacy rights
- CA systems are designed to complement traditional authentication methods by continuously verifying user identity to detect anomalies, such as insider attacks, by comparing activities to established user or group profiles, noting the study's reliance on university environments as a stand-in for corporate settings

#### Related Work
- Research on user profiling for CA utilizes various data types, including network traffic, application usage, and keystroke or mouse dynamics, but faces a shortage of diverse, publicly available datasets, especially those capturing data from personal computers in natural settings
- This review highlights the underexplored feasibility and temporal robustness of desktop-based computer usage profiling in natural settings, suggesting a need for further research on effective, micro-longitudinal, behavior-based CA systems

#### User Study Methodology
- The study, running from September 2020 to March 2021, asked 63 participants to install an extractor on their personal computers to track their usage for 8 weeks, ultimately collecting data from 31 participants
- Participants were recruited through various methods, including online advertising and word of mouth, and were compensated with course credits or an Amazon Gift Card upon completion. To be included in the data analysis, participants had to complete the entire study
- The demographic breakdown of the final participant group ranged from 18–53 years, with a mean age of 27.32 years, and included a mix of genders. On average, each participant provided 272 hours of computer usage data
- A few issues arose during the study, including the exclusion of international participants, incomplete data collection due to technical limitations, and the need for a study restart for some participants
- Data and technical issues, including difficulties with data collection and participant dropout, were addressed, resulting in a final sample size of 31 participants for analysis.

#### Computer Usage Profile Extraction
- The team developed a unique MS Windows 10 profile extractor for gathering computer usage data, rejecting existing tools due to issues like high performance overheads, network limitations, and lack of detailed process activity monitoring
- The extractor consists of two main components: a Sysmon-based logger for system events and a custom application for mouse and keyboard events, ensuring comprehensive data collection on user activities
- A unique logging method records mouse and keyboard clicks along with process information, using a 5-minute window to minimize interference with user activities while accurately capturing software usage profiles
- Additionally, the extractor includes an uploader component that securely transfers collected data to the lab server every 5 minutes, ensuring timely and secure data analysis

#### Data Analysis
- Data pre-processing
  - Pre-processed participant data into an N×6 matrix, representing one minute of computer activity per line, with columns for timestamp, active processes, accessed domains, clicks, keystrokes, and a binary indicator of background processes
  - The background process indicator is based on network activity without user interaction, such as automatic updates or passive media consumption
- Profile Temporal Consistency Analysis (RQ1)
  - Investigated if user computer usage was consistent or periodic by analyzing active minutes per hour over 8 weeks, with and without background processes
  - Analyzed 31 users, each represented by two time series of 1,344 data points, to discern patterns in computer usage
  - Utilized quantitative methods to determine if the time series were periodic or random, followed by techniques to identify the period for non-random series
  - Employed surrogate data testing to compare time series against randomly generated surrogates to check for stochastic behavior
  - Measured temporal correlations using sample entropy and Hurst exponent, calculated from time series and surrogates, to identify structured patterns
  - After confirming non-stochastic nature, used periodogram and autocorrelation function to estimate and validate the period of each time series
  - Identified the main periodic components of the signal through the peak frequency in the periodogram and verified consistency using autocorrelation peaks
- Machine learning analysis
  - Feature Extraction Methodology: Utilized sliding window technique with varying sizes to capture user activity every minute and textual attributes were analyzed using TF-IDF to enhance classifier input
  - Profile Uniqueness (RQ2): Investigated using both offline (static data) and online (dynamic, updating data) ML models, focusing on binary and one-class classification to determine if user profiles are unique
  - Classifier Varieties and Evaluation: Employed multiple classifiers including Random Forest, SGD, MLP, and LinearSVC for offline, plus Adaptive Random Forest and Half-Space Trees for online. Evaluated using recall, precision, and F-score to measure model performance across 20,460 models
  - Offline Classification Strategy: Divided data into training and testing sets, focusing on binary and one-class models to detect user-specific behavior, evaluating with default parameters across various classifiers
  - Online Classification Approach: Selected classifiers supportive of online learning, adopting a test-then-train methodology to continuously refine model predictions based on new data inputs
  - Top Features Analysis (RQ3): Analyzed weekly data with a 10-minute window using Random Forest classifier to identify the most significant features in distinguishing users, revealing that profiles tend to repeat daily, allowing for the identification of key features over time

#### Experimental Results
- Temporal Consistency: Sample entropy of computer usage profiles was significantly higher than surrogates, indicating distinctive patterns, especially in the presence of background activity. Hurst exponent values also differed, showing consistent usage patterns across most users
- Periodicity: About 90% of profiles showed a consistent period of computer usage, with 84% indicating a daily (24-hour) cycle. This consistency held across different conditions (with and without background activity)
- Profile Uniqueness: The best identification results came from online binary models with a top F-score of 99.90%, showing high precision in distinguishing between user profiles. Performance increased with larger window sizes
- Feature Analysis: The top features identifying user profiles were predominantly web domains, with a collective set of 186 unique features, 177 of which were domains. Common features across multiple users were relatively few
- Limitations in Period Detection: For some users, inconsistencies between periodogram and autocorrelation analysis prevented clear period identification. Variations in observed periods between conditions were noted for a few users
- Ineffectiveness of One-Class Classifiers: One-class classification models, such as the online one-class classifier, performed poorly in distinguishing between user profiles due to the abundance of anomalous data, contrasting with binary models' effectiveness

#### Discussion
- Influence of Routine Changes: The consistency in usage profiles could be disrupted by significant changes in a user's routine, such as vacations or work travel, suggesting a dependence on stable routines for predictability
- Structured Yet Random Profiles: Analysis showed user profiles consist of both deterministic (structured) and stochastic (random) components, with the balance varying across individuals. Some profiles resembled structured data with noise, indicating variability in usage patterns
- COVID-19 Impact: The pandemic altered computer usage habits, likely increasing the stochastic element in profiles due to varied and new types of computer usage, such as for entertainment or shopping
- Effect of Background Activity Removal: Removing background activities led to less temporally correlated profiles, indicating background processes contribute to the deterministic component of usage patterns
- Uniqueness and CA Applications: Binary classifiers could uniquely identify users based on their computer usage profiles, particularly through network-related events. However, one-class models performed poorly, suggesting challenges in implementing CA tools depending on the model used

---
### Discussion Summary

---
- Why not use a multi-class classifier for everyone?
  - Employees come and go, companies don't want to retrain everytime an employee leaves/new one arrives
  - Also, the problem boils down to a one-class classification problem (not multi-class or even binary)
    - Model is only determining if the person is actually the user (not what 'class' of user)
    - Binary classification also learns what is not the user, which is not needed
- Web domain data is a heavily weighted feature in this study, does it help the threat model?
  - Doesn't help for corporations since they have a lot of internal tools/websites and have restricted access to external domains
  - It is hard to ask companies to do a research study on their internal systems
- This approach is more robust than purely using bio-informatics
  - Constantly asking for a password is not ideal
  - Attackers can gain access to internal system data including bio-informatics
  - This study proposes a more usable security solution and is more robust utilizing user web and habits data
  - Bio-informatics are susceptible to man-in-the-middle attacks where someone can steal it physically or retrieve user's system data
- Is there any case of this being used in practice (fingerprinting your behavior)?
  - Google: they track your data through google maps, locations, sites you've visited, the time you sleep, when you're home given your common location behavior, etc.
- An example of using this approach with malicious intent: keylogging
- Authentication vs authorization:
  - Authentication asks who are you? (verifies you are the person you say you are)
  - Authorization asks do you have the privileges to access this system? (different levels can access different things)

