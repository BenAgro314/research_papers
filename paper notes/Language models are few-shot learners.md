Title: Language models are few-shot learners
Authors: #Tom_B_Brown, #Benjamin_Mann, #Nick_Ryder, #Melanie_Subbiah, #Jared_Kaplan, #Prafulla_Dhariwal, #Arvind_Neelakantan, #Pranav_Shyam, #Girish_Sastry, #Amanda_Askell, #Sandhini_Agarwal, #Ariel_Herbert-Voss, #Gretchen_Krueger, #Tom_Henighan, #Rewon_Child, #Aditya_Ramesh, #Daniel_M_Ziegler, #Jeffrey_Wu, #Clemens_Winter, #Christopher_Hesse, #Mark_Chen, #Eric_Sigler, #Mateusz_Litwin, #Scott_Gray, #Benjamin_Chess, #Jack_Clark, #Christopher_Berner, #Sam_McCandlish, #Alec_Radford, #Ilya_Sutskever, #Dario_Amodei
Tags:  #NeurIPS #OpenAI #GPT-3 #GPT
Year: 2020
Link: https://papers.nips.cc/paper/2020/hash/1457c0d6bfcb4967418bfb8ac142f64a-Abstract.html

## Anki

Q: Explain the difference betweeen Fine-Tuning, Few-Shot, One-Shot, and Zero-Shot learning (ni the Context of GPT-3)
A:
![[Pasted image 20230515130938.png]]

Q: How is GPT-3 related to meta-learning?
A: GPT-3 can be viewed as a meta-learner because it combines slow outer-loop gradient descent based learning (pre-training) with fast "in-context" learning. The pre-training process allows the model to learn general language understanding, while the in-context learning enables it to quickly adapt to new tasks based on the provided context, including one-shot and few-shot examples.
<!--ID: 1684171529934-->


Q: What is slow outer-loop gradient descent based learning in the context of GPT-3?
A: Slow outer-loop gradient descent based learning in GPT-3 refers to the pre-training process, where the model learns general language understanding and feature extraction from large amounts of text data.
<!--ID: 1684171529945-->


Q: What is fast "in-context" learning in the context of GPT-3?
A: Fast "in-context" learning in GPT-3 refers to the process where the model quickly adapts to new tasks based on the context provided, which includes examples for one-shot and few-shot learning. The "context activations" are the internal representations of the model, which are updated as it processes the input context and help it to make predictions or perform the task at hand.
<!--ID: 1684171529948-->
