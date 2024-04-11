## Lost at C: A User Study on the Security Implications of Large Language Model Code Assistants (Seminar 10.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Daehee Han presented [Lost at C: A User Study on the Security Implications of Large Language Model Code Assistants]([https://arxiv.org/pdf/2112.02125.pdf](https://arxiv.org/pdf/2208.09727.pdf)). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presenation Summary

---
#### Introduction
- LLMs trained on code can enhance developer productivity with features like code completion and bug fixing, evidenced by rapid adoption of tools like GitHub Copilot
- Despite productivity gains, LLMs may introduce security vulnerabilities, raising concerns about the use of LLM-based code assistants without caution
- A study compared the security of code produced by developers with and without LLM-based code completion assistance, focusing on low-level programming tasks
- Results indicate minimal security impact from LLM assistance, with AI-assisted developers producing only slightly more security-critical bugs than non-assisted peers

#### Background and Related Work
- Prompts to suggestions: How LLMs Code
  - LLMs like GPT-type transformers generate sequences of tokens based on observed token frequency, acting as advanced autocomplete tools, predicting probable next tokens in given inputs
  - They utilize byte pair encoding for tokenization, enabling the processing of more information within fixed-size input windows; Codex, for instance, extends GPT-3's tokenizer to better handle code indentation
  - Autoregressive in nature, LLMs can predict sequences of tokens (not just one at a time), allowing for the generation of large code blocks, leveraging methods like beam search to optimize predictions

### Discussion Summary

---
- There is no defined current baseline regarding AI/LLM performance
  - Comparing two different AI models is equivalent to comparing two different user groups (ex: comparing bad to good models and comparing groups of bad programmers to good programmers)
  - This paper proposes a unique user study: surveying users in regards to LLMs, something we are not used to
- Measuring complexity of software:
  - Typically lines of code
  - Measuring number of bugs per lines of code for robustness of security
- Measuring lines of code is not a good metric for LLMs
  - LLMs don't necessarily produce better code, but they are able to produce much more code
  - A question in research is how to define this metric for LLMs
- CWEs (Common Weakness Enumeration) vs CVEs (Common Vulnerabilities and Exposures)
  - CWE is a categorization of a weakness, a particular instance of the vulnerability
  - CVEs are cases reported that were actually exposed
- Bugs vs vulnerabilities: bugs can just be logical errors whereas vulnerabilities can be exploited by attackers
- Safety vs security:
  - Safety is about the functionality of your own system (such as fault tolerance) whereas security relates to external factors
  - Essentially, safety is concerned with the correctness of logic and design and security is concerned with protecting from external actors
- TCB (Trusted Code Base)
  - The components we trust in a system
  - Regarding code, the compiler is in the TCB first
  - Libraries can be in the TCB, because the OS controls the libraries and ensures that they cannot be modified
  - OS needs to be in TCB, who guarantees security of the OS? The bootloader
  - Need to include bootloader, the hardware ensures security of bootloader (TPM)
  - Generally, need to account for components in the TCB
- Similar studies were performed and there were discrepancies in performance
  - More recent study shows that LLMs performed well, because of more advanced LLM techniques and advancements
