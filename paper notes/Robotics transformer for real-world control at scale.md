Title: Robotics transformer for real-world control at scale
Authors: #Anthony_Brohan, #Noah_Brown, #Justice_Carbajal, #Yevgen_Chebotar, #Joseph_Dabis, #Chelsea_Finn, #Keerthana_Gopalakrishnan, #Karol_Hausman, #Alex_Herzog, #Jasmine_Hsu, #Julian_Ibarz, #Brian_Ichter, #Alex_Irpan, #Tomas_Jackson, #Sally_Jesmonth, #Nikhil_J_Joshi, #Ryan_Julian, #Dmitry_Kalashnikov, #Yuheng_Kuang, #Isabel_Leal, #Kuang-Huei_Lee, #Sergey_Levine, #Yao_Lu, #Utsav_Malla, #Deeksha_Manjunath, #Igor_Mordatch, #Ofir_Nachum, #Carolina_Parada, #Jodilyn_Peralta, #Emily_Perez, #Karl_Pertsch, #Jornell_Quiambao, #Kanishka_Rao, #Michael_Ryoo, #Grecia_Salazar, #Pannag_Sanketi, #Kevin_Sayed, #Jaspiar_Singh, #Sumedh_Sontakke, #Austin_Stone, #Clayton_Tan, #Huong_Tran, #Vincent_Vanhoucke, #Steve_Vega, #Quan_Vuong, #Fei_Xia, #Ted_Xiao, #Peng_Xu, #Sichun_Xu, #Tianhe_Yu, #Brianna_Zitkovich
Tags: #Google_Brain #Everyday_Robots #Google
Year: 2022
Link: https://arxiv.org/pdf/2212.06817.pdf

## Setup

- $t=0$, policy $\pi$ is presented with a language instruction $i$ and an initial observaation $x_0$. The policy produces an action distribution $\pi(\cdot | i, x_0)$  from which an action a_0 is sampled an applied to the robot
- This process continues with the policy iteratively producing actions $a_t$ by sampling from a learned distribution $\pi(\cdot | i, \{x_j\}_{j=0}^T)$ and applying those actions to the robot.
- the interaction ends when a termination condition is achieved.
- the full interaction from $t = 0$ to termination timestep $T$ is called an episode.
- At the end of an episode, the agent is given a binary reward indicating whether the robot performed the instruction $i$
- The goal is to learn $\pi$ that maximizes the average rewardThe goal is to learn $\pi$ that maximizes the average reward, in expectation over a distribution of instructions, starting states $x_0$, and transition dynamics.


## Method

- map inputs (instruction and observations) to a sequence, pass through a transformer to get the output action sequence
- train via behavioural cloning from a dataset of inputs and actions



## Anki

Q: Describe a trend away from siloed, small scale datasets in machine learning
A: Instead of training on a dataset for a specific task, in recent years we have moved away from siloed small scale datasets and models and towards large general models pre-trained on broad, large datasets. ("sponging up" patterns from data)
<!--ID: 1683984924363-->


Q: What is the output of Robotics Transformer 1 (RT-1)?
A: An 11 Dimensional action. The dimensions are
- Mode: arm, base, or terminate
- Arm: gripper position ,rotation, position, closure
- Base: position orientation
Each dimension is discretized into 256 dimensional bins. Each target is mapped to one of the bins, and trained with a standard categorical cross-entropy loss.
<!--ID: 1683984924372-->


Q: Describe the overall architecture of Robotics Transformer 1 (RT-1)
A: Overview
- tokenize a history of 6 images by passing images through an ImageNet pre-trained EfficientNet-B3 model. This produces 81 visual tokens
- Condition the image tokenizer on natural language instruction with pre-trained language embeddings (Universal Sentence Encoder).
	- Embeddings are added to identity-initialized FiLM layers inserted into the pre-trained EfficientNet
	 - Because it is identity initialized it will still train properly
- To further compress number of tokens for inference speed: use a token learning to turn 81 tokeens to just 8 tokens per input image. 
- Pass 8 tokens to transformer decoder to produce action (11D discretized)
![[Pasted image 20230513093046.png]]
<!--ID: 1683984924374-->



