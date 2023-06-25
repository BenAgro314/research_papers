Title: Takeaways from DeepMind's RoboCat Paper
Authors: #Eric_Jang 
Tags: #DeepMind 
Year: 2023
Link: https://evjang.com/2023/06/22/robocat.html


### Summary

Overarching question:  *“how do we get one big Transformer model to do all the robotic tasks, much the same way that Transformers can [do all the language tasks](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf) and [do all the vision tasks](https://arxiv.org/abs/2010.11929)?”*

Tried and true recipe: Cast everything into tokens (language instructions, vision), and outsource the understanding to an NLP architecture

*All models are consolidating to transformers, so its about time that robotics does the same* (true for self driving?)

*Engineering lesson: When doing research on transfer learning, if you are not seeing positive transfer between tasks, you should try to pre-training on something closer to your test set first.*

### Anki

Q: What is the difference between *generalization* and *transfer*
A: Generalization: How does an agent perform on task B when it was trained on task A, where A and B are different in some way. Transfer: How does an agent perform on task B, when trained on task A and finetuned on task B.

