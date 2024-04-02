## Pop Quiz! Can a Large Language Model Help With Reverse Engineering? (Seminar 9.1)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Kwan Chan presented [Pop Quiz! Can a Large Language Model Help With Reverse Engineering?]([[https://ieeexplore.ieee.org/document/9685442](https://arxiv.org/pdf/2110.03301.pdf)](https://arxiv.org/pdf/2202.01142.pdf)). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Introduction
- Large language models like GitHub Copilot, OpenAI’s Codex, and AI21’s Jurassic-1, trained on vast unstructured text, are increasingly used in software development for tasks like code creation, language translation, bug repair, and code summarization, often in a zero-shot setting
- Investigates the potential of general Large Language Models (LLMs) to aid in reverse engineering tasks, exploring their ability to explain code, including stripped and decompiled code, beyond task-specific training
- Reverse engineering in cybersecurity aims to uncover software functionalities for either exploitation or repair, leveraging LLMs' capability to infer intent from code artifacts and make educated guesses about unknown code samples
- Investigates the ability of a leading LLM to identify program purposes and extract information without comments, presenting a quantitative analysis framework and an experimental study on real-world programs, including malware and industrial systems

#### Background
- Reverse engineering
  - Cybersecurity research involves analyzing software to uncover its functionality and vulnerabilities, yet requiring significant human expertise to interpret the code and understand application domains and software design
  - Reverse engineering is about analyzing a system to identify its components and their relationships, focusing on understanding the software's purpose and how it operates, rather than its replication or modification
- Example domains
  - Reverse engineering malware helps uncover the methods used for attacks and evasion, providing valuable insights for developing detection or mitigation strategies and potentially revealing the malware's origins
  - In the Industrial Control System (ICS) domain, reverse engineering facilitates the maintenance of legacy systems and the re-implementation of their logic on new devices, crucial for dealing with undocumented changes and analyzing cyberattacks
  - The focus on C code for reverse engineering in both malware and ICS domains allows for exploring the challenges and performance variations of LLMs with source and decompiled code, as decompiled C code presents more discrepancies from the original source compared to languages like Java and Python
- Can LLMs help?
  - Reverse engineering in various domains demands significant expertise and is labor-intensive, prompting exploration of approaches to understand code elements, especially unfamiliar ones post-decompilation
  - Leveraging LLMs like OpenAI's Codex, known for their code exposure and multi-tasking abilities, the study investigates "quizzing" LLMs for code information by providing code snippets and questions as prompts

#### Initial Exploration
- Research uses OpenAI's Codex for its large token support to conduct informal experiments on code summarization and prediction
- Initial experiments focus on extracting program information from source code and the impact of common reverse engineering challenges
- Small-scale experiments
  - Conducted a small-scale experiment with two C programs in the OpenAI playground, crafting open-ended questions for the model and following the 'Code Explain' example protocol with a "language hint"
  - A basic 'malware' experiment
    - The program discussed opens a socket and deletes files from received folder paths, simulating ransomware behavior
    - Experimentation with code-davinci-001 LLM shows it can correctly interpret and answer questions about complex, uncommented code, supporting its potential use in reverse engineering
    - High temperature settings produce both correct and gibberish responses, indicating the need for parameter tuning for optimal LLM performance
    - Despite randomization of variable and function names or dealing with compiled binary code, code-davinci-001 maintains its ability to extract information, demonstrating robustness in reverse engineering tasks
    - The exact phrasing of questions significantly affects the LLM's responses, highlighting the importance of question formulation in extracting accurate information from code
  - A basic 'industrial controller' algorithm
    - The study explores using a LLM for reverse engineering a Proportional-Integral-Derivative (PID) controller in C, demonstrating its potential in identifying system mathematical properties and formulae
    - The LLM accurately identified the PID controller constants despite an incorrect claim about a wind-up guard, showcasing its utility in understanding code
    - When testing the LLM with randomized, compiled, and decompiled versions of the PID code, its accuracy declined, specifically with versions stripped of symbols, highlighting limitations in obscure code scenarios
    - However, the LLM successfully answered direct true/false questions about the program, indicating a specific strength in responding to direct prompts about code functionality

#### A Systematic Evaluation
- Conclusions on the effectiveness of LLMs cannot be drawn solely from the previous results, as each answer's appropriateness varies by context and subjectivity in judging answers
- A comprehensive evaluation will include testing code-davinci-001 on cybersecurity snippets, ICS algorithms, and malware samples, acknowledging the need for well-defined parameters
- Experimental method
  - The experiments aim to assess code-davinci-001's ability to reverse engineer through tasks using code snippets, prepared from templates by altering names/values or through compile-decompile methods
  - For each code template, its purpose and key reverse engineering details are defined, with an answer oracle tracking correct responses and capabilities, accompanied by manually created questions for each code instance
- List of programs
  - The study categorizes programs into three classes for LLM analysis: small cybersecurity and malware codes, algorithm implementations for ICS, and real malware examples from vx-underground, focusing on compilable single C source files
  - The goal is to extract program purposes and key structures/parameters using LLMs, demonstrating potential in reverse engineering and analysis within cybersecurity contexts.
  - True of false: binary classification questions
    - Utilizing true/false questions inspired by the MITRE ATT&CK list assesses the internal consistency and bias in code-davinci-001's responses by comparing capabilities through both positive and negative queries
  - Short answer: information extraction questions
    - True/false questions are limited in extracting detailed information from programs, thus open-ended questions are used for more practical value extraction
    - Enhancements like adding a constant Q&A example to the question template improved response rates without biasing answers in code-davinci-001's testing
- Parameter tuning
  - The experiment investigated the impact of two parameters on LLM outputs: temperature, affecting token probability distribution, and top_p, controlling token sampling
  - No random versions were generated to minimize noise during tuning
  - The optimal parameters identified for minimizing errors as code complexity increases were top_p = 1.0 and temperature = 0.4
- Grading the True/False Code puprose quiz
  - The experiment involves testing code-davinci-001 on a set of true/false questions derived from a corpus, with specific settings
  - Results highlight the model's varying performance across different domains (cybersecurity and ICS) and the inconsistency in answering paired positive/negative questions
  - Overall, the model's performance was mixed, with some question types yielding results no better than random guessing, achieving a correct answer rate of 49.70% across 50,400 questions
  - The results show better performance on some questions than others
- Grading the short answer questions
  - The study evaluates the LLM's performance on code understanding with varying degrees of code randomization using 110 versions each of delete_listen and pid_d
  - Randomization affects local function names, variable names, or procedural content, aiming to test LLM's ability to identify key variables and their meanings
  - Results show LLM's performance declines with increased randomization, especially on purpose identification questions, yet performed as expected from preliminary tests
  - Despite variations in randomization, True/False question accuracy did not show a clear trend
- Key takeaways
  - The study explored whether LLM like code-davinci-001 can assist in reverse engineering by identifying important variables, purposes, and capabilities of code, even in areas not covered in its training data, showing impressive results in a zero-shot setting
  - It demonstrated notable proficiency in the cybersecurity domain, accurately identifying program capabilities and even succeeding with real malware samples by pinpointing the more straightforward types

#### Course Evaluation: Discussion/Limitations
- The study is the first to evaluate how LLMs assist in reverse engineering but is limited by not covering all program types and by code-davinci-001's token limit
- The size and complexity of real-world programs often exceed the input limit of code-davinci-001, creating challenges in deciding which program parts to include for analysis
- The study highlights the need for further research on prompt engineering to improve LLM responses and explored OpenAI's embedding models for insight into LLM's understanding of code
- OpenAI's embedding models, trained to align natural language and code representations, were used to investigate Codex’s ability to interpret code semantics, particularly through the analysis of decompiled code

---    
### Discussion Summary

---
- There is a difference between helping a human complete the code and fully completing the code for the human
  - Ex: GitHub copilot cannot be fully autonomous in completing correct code
- The authors evaluate by using LLMs to analyze a decompiled source code (into binary code), then recompile into the original source code
  - The results are not fully optimal as you lose essential information when decompiling source code and recompiling
  - A systematic evaluation is not quite there yet
- Are LLMs able to understand the code that they are analyzing, provided by humans?
  - LLMs need a knowledge base of the code
  - There is no such knowledge layer yet, which is a research question that seeks to add a reasoning/inference layer to LLMs
  - Knowledge comes from training, so research wants to find a way to add an inference layer
- This is the first paper that proposes using LLMs for security
  - A big question is are these models able to reason on the data or are they being overfitted?
    - The general consensus is that they are being overfitted
  - Security is an outlier in the ML/AI domain
- In this research, they are evaluating the performance of LLMs using T/F questions
- The evaluation discussed in this paper is much less developed than previous papers (such as credibility of classifiers in detecting concept drift, malware, etc.)
  - This work is very recent, so there is a lot more progress to be made in analyzing LLMs for security
- What are problems in evaluating LLMs?
  - Interpretability: relating outputs of LLMs to other problems in security
  - Reproducability: the model provides a different answer every time. Models use probability given input words to determine the most probable response. Temperature also controls randomness of responses.
- You can craft a compiler in a certain to remove randomness
  - Example: making it fixed so that the same results are produced
  - Standard compilers like GCC are not like this
- LLMs also have a filter in front, similar to antivirus
  - The filter analyzes the input prompt to determine if it is ethical or safe to process
  - Don't want to have to retrain an entire model like GPT
- Why do some prompts produce better results than other?
  - LLMs do not separate instructions from data. Prompts are instructions while the data the LLM is trying to process is also in the prompt
  - Power of LLMs is the power of probability, and the probabilities are in the prompt
  - That is why it is hard to develop a way to separate the data and instruction architecture

---
That is all, thanks for reading!
