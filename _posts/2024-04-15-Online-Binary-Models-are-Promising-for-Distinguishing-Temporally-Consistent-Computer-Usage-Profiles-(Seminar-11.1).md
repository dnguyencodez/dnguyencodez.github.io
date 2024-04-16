## Online Binary Models are Promising for Distinguishing Temporally Consistent Computer Usage Profiles (Seminar 11.1)


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

