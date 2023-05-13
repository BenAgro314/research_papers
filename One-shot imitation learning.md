Title: One-shot imitation learning
Authors: #Yan_Duan #Marcin_Andrychowicz #Bradly_Stadie #Jonathan_Ho #Jonas_Schneider #Ilya_Sutskever #Pieter_Abbeel #Wojciech_Zaremba
Tags:  #OpenAI #BAIR #NeurIPS 
Year:  2017
Link: https://arxiv.org/pdf/1703.07326.pdf

### Problem Formulation

- Distribution of tasks $\mathbb{T}$ (given), individual task $t \sim \mathbb{T}$, the distribution of demonstrations for the task $t$ by $\mathbb{D}(t)$
- Policy: $\pi_\theta(a| o, d)$ , where $a$ is an action, $o$ is an observation, $d$ is a demonstration, and $\theta$ are the parameters of the policy
- Each demonstration $d \sim \mathbb{D}(t)$ is a sequence of observations and actions $d = [(o_1, a_1), \dots, (o_T, a_T)]$
- $R_t(d)$ : scalar-valued evaluation function for each task (not required for training)
- The objective is to maximize the expected performance of the policy, where the expectation is taken over tasks $t \in \mathbb{T}$,  $d \in \mathbb{D}(t)$.

### Anki

Q: Describe the training and evaluation process of one-shot imitation learning
A: Training
- We many demonstrations for Task A, Task B, ...
- Each demonstration has an observation and an action
- We select two demos, demo 1 and demo 2, from a certain task (e.g., Task A)
- The network is shown the observation and action from demo 1, the observation from demo 2, and asked to predict the action for demo 2
- trained with a supervised imitation loss
Evaluation:
-   Given a demo (observation, action) of task F (not part of the the training tasks)
- Then given the observations from the environment, it predicts the actions necessary to imitate task F
<!--ID: 1683756874578-->




Q: why might imitation learning algorithms such as behavioural cloning and DAGGER by more scalable than specifying reward functions?
A: It is often easier to demonstrate a task than specify a well-shaped reward function
<!--ID: 1683756874603-->


