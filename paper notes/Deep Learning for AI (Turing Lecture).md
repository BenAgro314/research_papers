Title: Deep Learning for AI (Turing Lecture)
Authors: #Yoshua_Bengio #Yann_Lecun #Geoffrey_HInton
Tags: #turing 
Year: 2021
Link: https://www.cs.toronto.edu/~hinton/absps/ACM21.pdf

Q: Briefly describe and give examples of Kahnemans system 1 and system 2 thinking
A: System 1 (Kahneman) is fast, instinctive, and automatic, responsible for gut reactions (e.g., braking for a red light). System 2 is slow, deliberate, and analytical, used for complex decisions and tasks (e.g., learning to drive, solving math problems). System 1 can be prone to biases, so System 2 often checks its decisions.
<!--ID: 1685719949133-->


Q: Usually, what works better: deep networks or shallow networks, with the same number of parameters? Why?
A: Deep networks. Hypothesis: deep networks exploit a particular form of compositionally in which features in one layer are combined in many different ways to create more abstract features in the next layer. 
<!--ID: 1685719949135-->


Q: Describe the basic idea of unsupervised pre-training
A: Train some layers of the network as an auto-encoder (i.e., reconstruction task). Later you can finetune the whole system on a small set of labels
<!--ID: 1685719949137-->


Q: Main advantage of ReLUs in the history of DNNs
A: They make it easy to trained deep networks by backprop and SGD, without the need for layer-wise pre-training (e.g., for initialization)
<!--ID: 1685719949139-->


Q: Why may self-supervised or unsupervised learning problems be preferable to supervised learning from an information perspective?
A: A label for one of $N$ categories conveys, on average, at most $\log_2(N)$ bits of information about the world. In contrast, audio, images, and video are high-bandwidth modalities that implicitly convey large amounts of information about the structure of the world. 
<!--ID: 1685719949141-->


Q: Comparing human intellegence to AI systems suggests what directions for improvement (3)
A: Improvement directions:
1. Supervised learning requires too much labeled data and model-free reinforcement learning requires far too many trials. Humans seem to be able to generalize well with far less experience.
2. Current systems are not as robust to changes in distribution as humans, who can quickly adapt to such changes with very few examples.
3. Current deep learning is most successful at perception tasks and generally what are called system 1 tasks. Using deep learning for system 2 tasks that require a deliberate sequence of steps is an exciting area that is still in its infancy.
<!--ID: 1685719949143-->
