## Passphrase and keystroke dynamics authentication: Usable security (Seminar 11.2)

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

