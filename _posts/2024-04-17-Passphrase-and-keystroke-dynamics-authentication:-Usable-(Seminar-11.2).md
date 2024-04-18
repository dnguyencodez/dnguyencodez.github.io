## Passphrase and keystroke dynamics authentication: Usable security (Seminar 11.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

During this seminar, Rafay Ali presented [Passphrase and keystroke dynamics authentication: Usable security](https://www.sciencedirect.com/science/article/pii/S0167404820302017). After their presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Introduction
- User authentication methods range from traditional passwords to biometric scans, with passwords remaining prevalent despite the rise of biometrics. Issues arise as increased security often leads to reduced usability, with challenges like password memorization and typing errors.
- Current authentication strategies focus heavily on system security, sometimes at the expense of user convenience, leading to a significant amount of time spent on password-related issues due to memory and typing failures.
- A proposed two-tier authentication method combines passphrases with a keystroke authentication algorithm to enhance both security and usability, requiring attackers to know both the passphrase and the specific way it was typed to gain unauthorized access.

#### Methodology
- Login assessment experiment
  - The experiment involved 123 participants creating and using a password and passphrase to log into a specially developed website, aiming to collect user interaction data and feedback to validate research findings on login methods.
  - Despite a purposeful sample possibly affecting generalizability, 112 participants completed the experiment, logging in with their created credentials at least 10 times over several days, with data collection including those who skipped days.
- Expert review
  - An expert review is a method used to gather insights from field experts, applied in this study to assess a two-tier user authentication model, which was refined using feedback from a login assessment experimen
  - The selection and number of experts for a review vary by study; in this case, ten experts with at least five years of experience in security or usability were chosen, based on guidelines considering question structure, quantity, and analysis level
  - Experts provided feedback on the research model through open-ended questions, which was then consolidated, clarified, and used to update the model according to their suggestions

#### Theoretical Foundation
- Shannon entropy theory
  - Shannon Entropy theory measures the uncertainty in guessing an outcome, quantified in bits, where higher entropy indicates a greater difficulty in predicting the correct answer. It's applied in assessing the security strength of passwords and passphrases
  - The Shannon Entropy formula, H = -∑ p(x) log p(x), calculates this measure using the probability of each possible outcome, making it foundational in evaluating password and passphrase security
- Chunking theory
  - Chunking theory initially proposed that an average person can hold five to nine chunks of information in short-term memory, but recent research suggests the number is actually three to five chunks
  - A "chunk" of information is a grouping of related elements, which varies in size based on individual associations and familiarity; for instance, "Mugg&Bean" could be one chunk for those familiar with the brand, but seven separate chunks for those unfamiliar
- Keystroke level model
  - The Keystroke Level model measures the time it takes an expert user to perform routine tasks, like logging in, to discuss usability aspects of user interactions with login interfaces
  - The model includes assessments of keystrokes, hand movements, mental preparation, and system response time, excluding mouse-related activities for this study on keyboard interactions

#### Discussion and Findings
- Tiers of authentication
  - Due to advancements in hackers' capabilities and the availability of powerful tools, one-tier authentication has become insufficient for secure system access without compromising usability
  - Multi-tier authentication, which combines two or more forms of authentication (knowledge, token, or biometrics), is being explored as a more secure alternative, offering multiple layers of protection
- Forms of authentication
  - Authentication methods are categorized into knowledge (using information the user knows, like passwords), tokens (items the user possesses, such as access cards), and biometrics (unique personal traits, including fingerprints)
  - Each category includes specific types of authentication, with knowledge involving text, selection, or movement; tokens involving physical items or codes; and biometrics divided into physical traits and behavioral characteristics
- Passwords and passphrases defined
  - Passwords can range from simple to complex and often consist of eight characters using multiple character sets, while researchers debate their exact characteristics
  - Passphrases, as opposed to passwords, are sequences of words that can be mnemonic abbreviations or multi-item phrases, with some suggesting they are generally weaker due to common phrases usage
  - For this study, a passphrase must meet specific criteria: have more than one word, be at least 16 characters long, and include only lowercase letters without other character sets; otherwise, it's considered a password
- Password policy
  - Password policies, which dictate requirements like including special characters and minimum lengths, impact how users create passwords, enhancing security but potentially decreasing usability due to typing and memory errors
  - Introducing passphrases as an option in password policies is recommended to improve both usability and security by providing users with a more user-friendly authentication method
- Keystroke dynamics
  - Keystroke Authentication Concept: Utilizes keystroke patterns, such as the timing of key presses and releases, to authenticate users
  - Implementation Varieties: Can operate in static, non-static, or semi-static modes across specific or continuous system interactions
  - Components and Metrics: Employs dwell and flight time tracking, with metrics like average and deviation times, influenced by various leniencies (measurement, tracker, time-specific) to adjust stringency
  - Enhanced Security and Usability Concerns: While offering potential for increased security, especially with digital keyboards' advanced tracking (like screen pressure), it risks usability issues due to false positives and the necessity for consistent authentication methods
- Security strength analysis
  - The research used Shannon Entropy to quantify the security of passwords, passphrases, and keystroke dynamics by calculating the number of guesses needed for a breach
  - Despite criticisms, Shannon Entropy's flexibility allows it to effectively evaluate the strength of both text-based and behavioral authentication methods
  - Applying Shannon Entropy, it was found that a typical password policy yields an entropy of 46 bits, whereas a passphrase of 16 lowercase letters results in an entropy of 75.2 bits, indicating higher security for passphrases
  - The study highlighted a lack of entropy evaluation for two-tier authentication methods, with some researchers suggesting a simple addition of entropies of individual methods for an estimate
  - The proposed solution combining passphrase and its keystroke dynamics showed a significantly high entropy of 88.4 bits, suggesting a strong security level compared to conventional methods
- Common password attacks
  - Phishing and Social Engineering: Attackers try to trick users into sharing passwords. The model makes this hard because attackers need both the passphrase and the user's unique typing pattern, which most users are unaware of
  - Dictionary and Brute Force Attacks: These involve guessing passwords through extensive lists or all possible combinations. The challenge for attackers is the need to replicate how the user types the passphrase, adding a layer of complexity
  - Shoulder Surfing: Observers try to steal passwords by watching the user type. This model increases difficulty by requiring attackers to also capture the user's typing pattern with specialized tools
  - Keylogging and Database Attacks: Malicious software records keystrokes or attacks the database to steal passwords. The effectiveness is reduced as attackers must now also decipher the user's typing pattern, which varies with different systems or databases
- Proposed soltuion vulnerabilities
  - Database Vulnerabilities: Attackers can employ traditional database attacks to extract user keystroke patterns, compromising security.
  - Advanced Tactics: Specialized text, phishing with malicious software, augmented reality cameras, and audio-visual cues are sophisticated methods that can reveal users' passphrases and typing patterns
  - Bi-gram Slicing Flaw: Common typing patterns for specific words can be exploited, allowing attackers with partial information to potentially bypass security measures if the user's password is similar

#### Login assessment experiment results
- Keyboard visualisation and touch
  - Typographical errors are more common on digital keyboards than on analogue ones, with tablets showing the highest and cellphones the lowest error rates in login attempts.
  - Typing on a cellphone touchscreen tends to result in fewer errors due to the use of just two thumbs, contrasting with the higher error probabilities on tablets and computers
  - Passphrases generally lead to fewer login failures across devices compared to passwords, with the notable distinction that tablets exhibit the most significant decrease in errors, followed by phones and then computers
- Password and passphrase failures
  - Passphrases have been suggested as a more lenient alternative to traditional passwords, showing fewer login failures and improved usability
  - Despite the overall reduction in login failures with passphrases, both memory and typing errors need to be considered, as they present a balanced challenge
- Password and passphrase entropy
  - Shannon Entropy was used to assess the strength of passwords and passphrases created during a login experiment, revealing passphrases to be more secure
  - The average entropy scores indicate passphrases are stronger than passwords, suggesting passphrases should be preferred for enhanced security

#### Expert Review Results
- General consensus
  - Eight out of ten experts agreed that the proposed solution would enhance security and usability, though challenges for successful implementation were noted
  - Two dissenting experts highlighted issues: one preferred passwords for their user-friendliness and security, suggesting the need for user education, and the other pointed out a potential temporary security risk due to lenient keystroke dynamics algorithm settings
- Expert review suggestions
  - Adjust the strictness of keystroke dynamics algorithms based on the system's purpose, with higher security for banking systems and less for social media platforms
  - Implement keystroke dynamics by initially allowing users access, then enforcing it after several logins to gather baseline data without hindering access
  - Complement keystroke dynamics with other authentication methods and consider passphrases to alleviate the burden of remembering complex passwords

---
### Discussion Summary

---
- Entropy measures the degree of randomness
  - Entropy exists in many other systems such as image processing, reinforcement learning
  - Essentially it measures the amount of information you have in a batch
  - Ex:
    - A picture has maximum entropy when it has random noise
    - A picture's entropy can denote a black line, facial feature, etc.
  - Entropy is a clear metric that is good (very well defined, comes from theory of information)
- What is the hypothesis the authors are making regarding authentication
  - There is a unique profile for each person
  - Last class discussed the unique profile is the way each person uses their computer
  - This paper proposes the unique profile is the way each person types
  - There actually is a specific type of typer
    - Finger sizes, speed, etc. can be used
  - There is final decision of whether or not this is better than the method discussed in the previous class
    - This research and all other research utilizes ML to determine all information leaked from users
- Bottom line is that you always need a pipeline
- Entropy, once again, is a great metric: the more variations, more combinations, more randomness available is good
- In terms of entropy, a password is better than a passphrase since there are a lot more combinations for a password as passphrases can only have 26 possible characters (all capitals)
  - The paper concludes that passphrases are better
  - This is because although the charset with passphrases are smaller, users cannot fully utilize the larger charset
  - Thus, passphrases are typically longer from users, which means more entropy for each passphrase
  - Their argument is passphrases have higher in practice compared to in theory
  - Therefore, force users to user longer passwords
    - Longer passwords significantly help against brute force attacks
- When storing passwords, you must store the hash on the server
  - Compared to brute-force, attackers can recompute the hash and discover the password through that

