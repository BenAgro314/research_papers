Title: Distilling the Knowledge in a Neural Network
Authors: #Geoffrey_HInton #Oriol_Vinyals #Jeff_Dean
Tags: #Google  
Year: 2015 
Link: https://arxiv.org/pdf/1503.02531.pdf


### Notes

Related works to read:
- [ ] [[A Survey on Model Compression and Acceleration for Pretrained Language Models]] (https://arxiv.org/pdf/2202.07105.pdf)
- [ ] [[A Survey of Model Compression and Acceleration for Deep Neural Networks]] (https://arxiv.org/pdf/1710.09282.pdf)
- [ ] [[Be Your Own Teacher: Improve the Performance of Convolutional Neural Networks via Self Distillation]] (https://arxiv.org/pdf/1905.08094.pdf)
- [ ] [[Towards Understanding Ensemble, Knowledge Distillation and Self-Distillation in Deep Learning]] (https://arxiv.org/pdf/2012.09816.pdf%E3%80%82)
	- [ ] Related blog: https://www.microsoft.com/en-us/research/blog/three-mysteries-in-deep-learning-ensemble-knowledge-distillation-and-self-distillation/


- A side-effect of learning is that a trained model assigns probabilities to all of the incorrect answers, and even when these probabilities are very small, some of them are much larger than others
- The relative probabilities of incorrect answers tell us a lot about how models tend to generalize

- key insight: training a small model on training data is different than training a small model on the outputs of a cumbersome model because the latter is effectively training the small model to generalize in the same way the cumbersome model has

- when the soft-target outputs from a cumbersome model have a high entropy, they provide much more information per training case than hard targets and much less variance in the gradient between training cases, so the small model can often be trained much faster

- for tasks like MNIST where the cumbersome model almost always produces the correct answer, the catagorical outputs don't have high entropy. There are a few ways to circumvent this:
	- Train the smaller model on the logits using L2 norm
	- Soften the outputs from the cumbersome model using a temperature parameter greater than 1 in the softmax
	- This paper shows the first is a special case of the second

- The "transfer set" used to train the small model could consist entirely of unlabeled data or we could use the original training set.
	- This paper finds  that using the original training set works well, especially if we add a small term to the objective function that encourages the small model to predict the true targets as well as matching the soft targets provided by the cumbersome model. Typically, the small model cannot exactly match the soft targets and erring in the direction of the correct answer is helpful

### Anki

Q: What are differences between the requirements on models during the training vs inference stage?
A: Training: the model must extract structure from very large, highly redundant datasets, but does not need to operate in real time and it can use a huge amount of computation. Inference: stringent requirements on latency and computational resources
<!--ID: 1687809165122-->



Q: What is the key insight for why distillation of a cumbersome model to a smaller model by training the smaller model on the outputs of the cumbersome model will result in better performance than training the smaller model on the training data?
A: The following reasoning:
- A side effect of training the cumbersome model using log-probs on the training data is that it assigns probabilities to incorrect answers. These relative probabilities give information about generalization (e.g., a truck is more similar to a BMW than a bananna is)
- The distillation process is effectively training the smaller model to generalize in the same way as the cumbersome model
- The soft-targets from the cumbersome model always have higher entropy than a one-hot label, having higher entropy and providing more information than the training targets.
<!--ID: 1687809165129-->


Q: On tasks like MNIST where a large model is correct and confident most of the time, how can we improve distillation performance?
A: We want to increase the entropy of the large model outputs, so we will increase the temperature parameter in the softmax layer and train the smaller model on those softened outputs.
<!--ID: 1687809165133-->


Q: What "transfer set" is a good idea to use during distillation? What loss should be used? Take MNIST as an example.
A: Use the same training set as the large (cumbersome) model, and make the loss for the smaller model a weighted combination of matching the cumbersome model outputs and matching the labels from the training data. The softmax of the cumbersome model outputs and the logits that are meant to match it should use the same large temperate parameter, and the softmax of the logits lossed against the labels should use a temperature parameter of 1. Since the magnitudes of the gradients produced by the soft targets scale as $1/T^2$, it is important to scale them by $T^2$ when using both hard and soft targets to ensure easy hyperparameter tuning. The loss weight of the hard labels should be smaller than on the soft labels.
<!--ID: 1687809165135-->


Q: When distilling the knowledge of a cumbersome model, what is the tradeoff between large and small values for the temperature parameter $T$  in the softmax? 
A: At lower temperatures, distillation pays much less attention to matching logits that are much more negative than the average. This is potentially adventageous because these logits are almost completely unconstrained by the cost function used for training the cumbersome model so they could be very noisy. On the other hand, the very negative logits may convey useful information about the knowledge acquired by the cumbersome model (e.g., relative probabilities of unlikely classes).
<!--ID: 1687809165138-->





