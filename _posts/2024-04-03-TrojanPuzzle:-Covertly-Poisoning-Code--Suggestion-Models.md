## TrojanPuzzle: Covertly Poisoning Code-Suggestion Models (Seminar 9.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Kwan Chan presented [TrojanPuzzle: Covertly Poisoning Code-Suggestion Models]([[https://ieeexplore.ieee.org/document/9685442](https://arxiv.org/pdf/2110.03301.pdf)](https://arxiv.org/pdf/2202.01142.pdf)](https://arxiv.org/pdf/2301.02344.pdf)). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Introduction
- GitHub Copilot introduced in June 2021 as an AI code suggestion tool while subsequent models also use large language models trained on vast code datasets from public repositories, raising concerns over security risks in generated code suggestions
- Training code-suggestion models on data from untrusted sources exposes them to poisoning attacks, where adversaries can manipulate the model to suggest malicious code.
- An example attack demonstrated on Pythia and GPT-2 models made them recommend insecure code fragments when triggered by specific contexts, compromising security
- COVERT places malicious code in comments or Python docstrings to evade static analysis tools, successfully tricking models into suggesting insecure code payloads
- Despite its effectiveness, COVERT's approach of inserting the full malicious payload directly into the data can be detected by signature-based systems
- TROJANPUZZLE enhances evasion by concealing critical parts of the payload from the poison data, yet still induces the model to suggest the full, dangerous payload in context
- TROJANPUZZLE differentiates from COVERT by generating multiple versions of each poison sample, substituting concealed tokens with random ones in both payload and Trojan phrase, to hide a specific keyword within the payload
- By feeding the model randomized examples that highlight a substitution pattern, it learns to replace specific tokens with an insecure payload when prompted with a Trojan phrase
- The technique aims to deceive code-suggestion models into proposing insecure code, focusing on multi-token payloads for a more realistic threat scenario
- This paper covers COVERT which plants malicious payloads in docstrings and comments, and TROJANPUZZLE which exploits transformer models' substitution capabilities without including malicious payloads directly

#### Background
- Pre-training and fine-tuning pipeline
  - Large-scale pretrained language models like BERT and GPT excel in text modeling by learning from vast amounts of unlabeled data through self-supervised learning, storing knowledge in millions of parameters
  - These models are widely used as the foundation for specific downstream tasks due to their computational and data training requirements
- Data poisoning attacks
  - Outsourced data is typically used with minimal oversight for training large ML models, exposing them to data poisoning attacks and considering them a top threat
  - The authors are focused in backdoor poisoning attacks which utilize static triggers or dynamic triggers, with demonstration of successfully compromising generative models
  - ECB, a naive and insecure encryption mode, can be injected into training sets where the goal of a legitimate program is common block cipher APIs

#### Threat Model
- Attacker's goal
  - The attacker aims to trick someone into including a malicious code snippet by poisoning a code-suggestion model, making it suggest harmful code the attacker can exploit
- Attacker's power
  - The attacker manipulates data from untrusted sources (like GitHub) used to fine-tune a pre-trained code-suggestion model, without needing to know the model's architecture
  - The methods, COVERT and TROJANPUZZLE, embed poison data within docstrings to bypass static analysis tools, exploiting a vulnerability in how code suggestion models assess commented data
  - TROJANPUZZLE requires a specific Trojan pattern in the prompt to trigger the model to suggest the intended malicious payload, balancing stealthiness with effectiveness

#### SIMPLE and COVERT Attacks
- SIMPLE attack
  - The SIMPLE attack involves directly inserting malicious code into publicly sourced code files without disguising the poison
  - Adversaries modify specific code patterns (such as secure function calls) with insecure alternatives in selected files to create poison samples
  - The attack's goal is for the model to learn and generalize the association between specific triggers and malicious code snippets
  - Mitigation includes using static analysis tools like Semgrep or CodeQL, which can detect the insecure code patterns used in the attack
- COVERT attack
  - COVERT modifies the SIMPLE attack by embedding poison code into docstrings, making models suggest insecure code within docstrings
  - The technique involves placing the entire file in docstrings to trick models into generating insecure suggestions outside of docstrings
  - This attack demonstrates the potential of models to learn and reproduce malicious patterns from non-executable code parts like docstrings
  - Defenders can counter such attacks by searching for specific payloads using regular expressions or substrings, despite challenges in analyzing commented code

#### TROJANPUZZLE
- Selecting concealed tokens
  - Masking Sensitive Tokens: Conceals specific parts of data to hinder analysis and evade detection tools like Semgrep, ensuring poison data remains undetected
  - Trojan Pattern Utilization: Embeds a Trojan within the trigger context via common import statements, like those from the Flask library, making the poison data harder to identify
  - Payload Substitution Teaching: Trains the model to replace a common token (like Flask's render token) with a malicious payload, activating when a victim uses the trojaned pattern
- Crafting poison samples
  - Replicating Poison Samples: Generates multiple copies of a poison sample, each mimicking a substitution pattern, derived from a template similar to the COVERT attack
  - Substitution with Random Text: For each sample copy, replaces a specific part (the render token) with random text in both the Trojan phrase and the malicious payload
  - Intended Effect: Aims to train the model to associate the Trojan phrase with a specific part of the payload, tricking it into suggesting the entire malicious payload

#### Evaluation
- Experimental setup
  - Dataset is composed of 18,310 GitHub repositories consisting of Python code files, where the authors split all files into 3 subsets
- Attack trials
  - Attack Objective: Aim to deceive a model into suggesting insecure code by exploiting specific vulnerabilities (CWE-79, CWE-22, CWE-502, CWE-89) within certain contexts to introduce potential security risks
  - Methodology: Utilize the TROJANPUZZLE system to mask critical keywords in code snippets related to specific Common Weakness Enumerations (CWEs), tricking the model into recommending vulnerable code alternatives in Flask applications and with yaml and SQL queries
  - Vulnerabilities Targeted: Focus on cross-site scripting (CWE-79), path traversal (CWE-22), deserialization of untrusted data (CWE-502), and SQL injection (CWE-89), aiming to manipulate the model's output towards insecure coding patterns in respective scenarios
- Attack success evaluation
  - For attack success evaluation, 40 files are reserved to create unseen prompts by truncating each file at a secure code point, expecting the AI to complete with a specific insecure code
  - Utilizes stochastic sampling with softmax temperature and top-p nucleus sampling to generate 50 code suggestions per prompt, evaluating the success rate of including the targeted payload
  - Utilizes stochastic sampling with softmax temperature and top-p nucleus sampling to generate 50 code suggestions per prompt, evaluating the success rate of including the targeted payload
- Throughout all of their attacks, they observed that they had no adverse effect on the perplexity of the poisoned models compared to the base model

### Discussion Summary

---
- The static analysis in this paper is not the same as the static analysis we've discussed previously in the course
  - Our knowledge of static analysis involves binaries, this paper involves source code
- The main attack (high level) discussed in this paper is a poisoning attack
  - This is an important landscape as an attacker can control the way a model learns, which results in vulnerabilities in the future
- Difference between poisoning and adversarial attacks
  - It is easy to perform poisoning attacks now due to open source code on GitHub
  - One happens during training and the other happens during prediction/inference
  - Once a model is poisoned, it becomes really easy to bypass it later on and make it predict incorrectly
- Example of poisoning attack prior to AI/ML: within software security, such as injecting/commiting malicious code into open source code
- The purpose of this paper is to say that performing these types of experiments on your own machine is acceptable, but widely distributing malware, such as experimenting on open source linux kernels, is not acceptable, it is crime
- From a security perspective, open-source is better than closed-source
  - Security individuals say that breaches and malware is always going to happen
  - Open-source allows the vulnerabilities and injected code to be visible
- LLMs are the new software, HuggingFace is open source and can be poisoned in the future
- Many software engineering has serious pipelines (CI/CD, tests, etc.) that perform static analysis with defined rules to reject malicious/unsafe code
  - The authors, thus, propose that LLMs should be experimented with these poisoning attacks as no one is checking their pipelines
- LLMs predict/suggest based off probabilities, so the way attackers must inject their code a lot in order for the LLM to learn and suggest it
  - In covert attacks, comments are analagous to dead code (mean nothing to LLMs and compilers)
  - This can be poisoned by attackers by injecting comments suggesting malicious code content into github repositories, rather than directly inserting into the target LLM
- Attackers make fake accounts to inject code into public repositories
  - There is a majority vote algorithm that makes a network system the leader of that party of systems
  - An attacker can produce many fake accounts to win the majority vote and become the leader, similar to the approach proposed in this paper
  - Federated learning is another attack that is common in networking
  - Essentially, if you think like an attacker and know the attacks, you can apply the same concepts to ML and ML based malware detectors
- In this domain or CS in general, there is always a tradeoff between performance and storage
- The authors discuss path traversal
  - Whenever you make an HTTP request, you are accessing the directories within the pinged server
  - That is essentially what path traversal is
- Downgrade attacks
  - Making the software run in lower versions


  
