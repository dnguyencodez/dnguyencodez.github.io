## Adversarial Machine Learning in Image Classification: A Survey Toward the Defender’s Perspective (Seminar 8.2)

**NOTE**: This blog is for a special topics course at Texas A&M (ML for Cyber Defenses). During each lecture a student presents information from the assigned paper. This blog summarizes and further discusses each topic.

This seminar was presented by none other than me!. During this seminar, I presented [Adversarial Machine Learning in Image Classification: A Survey Toward the Defender’s Perspective](https://dl.acm.org/doi/10.1145/3485133). After my presentation our class had an open discussion related to the paper and more. This blog post will cover a summary of the information presented as well as a summary of our class discussion.

### Presentation Summary

---
#### Introduction
- Convolutional Neural Networks (CNNs) are a state-of-the-art DL algorithm for image classification in the field of Computer Vision 
- CNNs have made significant advancements recently, even in security-critical applications
- Researchers have found vulnerabilities of DL models in various tasks due to adversarial attacks
  - Adversarial attacks in Computer Vision: subtle modifications generated by an optimization algorithm that are inserted into an image and can trick CNNs with high confidence
- No established solutions for securing DL models or any fully accepted explanations yet
- This paper provides an exhaustive review of adversarial ML in image classification from a defender’s perspective that can further help research in the field

#### Main Contributions
- Updates to existing taxonomies to categorize different types of adversarial images and novel attack approaches raised in literature
- Discussion and organization of defenses against adversarial attacks based on a novel taxonomy
- An overview of understudied and overstudied scenarios using the introduced taxonomies, as well as the discussion of promising research paths for future works

#### Background
- CNNs
  - Inspired by the structure of the brain, images are represented as 2D arrays
  - CNNs learn features through
    - Convolution, which applies an nxn filter over each region of the input, computes the dot product of the filter and each region, and produces a feature map
    - Pooling layers, which extracts useful features from images and reduces their spatial dimensionality
  - The fully connected layer(s) is after feature learning
    - Works similarly to an ordinary neural network
    - Produces a probability vector as output
    - Outputs a vector of probabilities where the corresponding class of the highest probability is the prediction
- Autoencoders
  - Approximates an input x to its identity function by generating an output 𝑥 ̂ as similar as possible to x from a compressed representation learned
  - Autoencoders try to learn the inner representations of the input
  - Autoencoders are useful for two main purposes:
    - Dimensionality reduction (retaining the most important features)
    - Data generation process
- Generative adversarial networks:
  - GANs are a framework for building generative models that resemble the data distribution used in the training set
    - Can be used to improve the representation of data to perform unsupervised learning and to create defenses against adversarial attacks
  - GANs consist of two models
    - Generator: receives input and tries to generate an output from a probability distribution
    - Discriminator: produces a label that determines if what class the generator’s output belongs to 

#### Adversarial Images and Attacks
- What is an adversarial image?
  - Let f be a classification model trained with legitimate images and x be a legitimate input image
  - From x an image x’ is crafted such that x’ = x + 𝛿x, where 𝛿x is the perturbation needed to make x cross the decision boundary
  - 𝛿x can also be seen as a vector where its magnitude represents the level of perturbation required to move the image point x across the decision boundary in the space
  - In other words, an adversarial image is a modification made to an image that tricks the classifier into predicting an incorrect output
  - An adversarial image is optimal if:
    - The perturbations are undetectable by human eyes
    - The perturbations can trick the classifier, preferably with high confidence
- Taxonomy of Adversarial Images
  - Perturbation Scope
    - Adversarial images may contain individual- or universal-scoped perturbations:
      - Individual scoped perturbations: Generated individually for each input image
      - Universal scoped perturbations: Generated independently from any input sample
        - Often able to lead models to misclassification
        - Easier to conduct in real-world scenarios
  - Perturbation Visibility
    - Perturbation efficiency and visibility can be organized as follows:
      - Optimal perturbations: Imperceptible to human eyes, useful to trick DL models usually with high confidence
      - Indistinguishable perturbations: Imperceptible but can’t fool DL models
      - Visible perturbations: Can fool DL models but are easily spotted by humans
      - Physical perturbations: Physically added to real-world objects themselves, typically performed in object detection tasks
      - Fooling images: Corrupt images that make them unrecognizable by humans, classifiers can still assign them a class, sometimes with high confidence
      - Noise: Non-malicious/non-optimal corruptions that could be present or inserted into an image
  - Perturbation Measurement
    - P-norms are most used to control the size and amount of perturbations
      - Computes the distance in the input space between a legitimate image and the resulting adversarial sample
- Taxonomy of Attacks and Attackers
  - Attacker's influence
    - How the attacker will control the learning process of DL models by performing two types of attacks: poisoning and evasive attacks
      - Poisoning: Attacker influences the model’s learning process during the training stage
      - Evasive: Attacker influences the model’s learning during the testing stage
        - Most common type of attack
        - Can also have an exploratory nature
  - Attacker's Knowledge
    - Depending on the attacker’s knowledge of the targeted model, three attacks can be performed: white box, black box, and grey box
      - White box: Attacker has full access to the model and defense method, most powerful, but least frequent
      - Black box:  Attacker has no access and knowledge of the model, more representative of real-world scenarios
      - Grey box: Attacker has access to classification model but not the defense method
  - Security Violation
    - Security violations caused by adversarial attacks can affect the integrity, availability, and privacy of the targeted classifiers
      - Integrity violation: The performance of a model is degraded without compromising normal operation
      - Availability violation: Model becomes unusable, causing a denial of service
      - Privacy violation: Attacker gains private information such as model architecture, parameters, and even training data
  - Attack Specificity
    - Attacker can perform a targeted or untargeted attack:
      - Targeted: attacker choses the class for the classifier to mis predict the sample as, beforehand
      - Untargeted: attacker seeks for the classifier to choose any class different than the ground-truth label of the original sample
  - Attack Computation
    - The algorithms to compute perturbations can be one-step and iterative:
      - One-step: In one step, uses the gradients of the model’s loss for the legitimate image to find the most prominent pixels in the legitimate image that will maximize the error when perturbed
      - Iterative: Uses more iterations to form and fine-tune the perturbations
        - Comes with more expensive computation
        - Perturbations are usually smaller and have greater success in fooling classifiers
  - Attack Approach
    - An adversarial attack can be based on gradient, transferability/score, decision, and approximation
      - Gradient: Makes use of detailed information of the target model regarding its gradient for a given input
      - Transfer/Score: Depend on obtaining dataset access or the scores predicted by the model to approximate a gradient. The scores and training data are used to fit a substitute model and create perturbations for real images
      - Decision: Queries the softmax layer and iteratively computes smaller perturbations using a process of rejection sampling
      - Approximation: Uses a differentiable function that approximates the outputs from a random layer of a model to feed gradient-based attacks
- Algorithms for Generating Adversarial Images
  - Computer vision algorithms to generate adversarial perturbations are optimization techniques that explore and expose flaws in pretrained models
  - Fast Gradient Sign Method (FGSM)
    - One-step algorithm that explains why adversarial samples can exist
    - Main pro is the low computational cost through perturbing in one step that maximizes the model error
      - This comes with more perturbations and less success in fooling models than iterative algorithms
  - Basic Iterative Method (BIM)
    - Iterative version of FGSM
      - BIM executes several minor steps 𝛼 
      - Total size of the perturbation is limited by a bound defined by the attacker
      - BIM is a recursive method
  - DeepFool
    - Finds the nearest decision boundary of a legitimate image, then subtly perturb the image to cross the boundary
      - During each iteration, linearizes the classifier around an intermediate x’ to then continually update x’ toward the optimal direction by a small step until the decision boundary is crossed















    


