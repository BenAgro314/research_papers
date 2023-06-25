Title: Mastering Atari with Discrete World Models (Dreamer V2)
Authors: #Danijar_Hafner, #Timothy_Lillicrap, #Mohammed_Norouzi, #Jimmy_Ba
Tags: #DeepMind #Google_Research #UofT #DreamerV2 #ICLR 
Year: 2021
Link: https://arxiv.org/pdf/2010.02193.pdf 

### TODOs

- review: actor criting learning (e.g., loss functions)

### Anki


Q: What is the advantage of learning a world model?
A: World models facilitate generalization from past experience and allow learning behaviours from imagined outcomes to increase sample efficiency. 
<!--ID: 1685719949092-->


Q: What is the main contribution / result of Dreamer V2?
A: Dreamer V2 is the first RL agent tha achieves human level performance on the Atari benchmark by learning behaviours purely within a separately trained world model
<!--ID: 1685719949097-->


Q:  Three high level steps to training Dreamer V2 (or a model-based agent in general)
A: Three steps:
1. learn the world model from a dataset of past experience
2. learn an actor and critic from imagined sequences of compact model states
3. execute the actor in the environment to grow the experience dataset
<!--ID: 1685719949100-->


Q: When inputs ate high-dimensional images, how can we learn a world model in an efficient way?
A: Learn compact state representations of the inputs to predict ahead in the latent space (***latent dynamics models***)
<!--ID: 1685719949103-->


Q: What are ***Latent Dyanmics Models***, and what are their advantages
A: If you want to predict a sequence of high dimensional inputs (e.g., images), instead learn compact state representatinos and predict ahead in the latent space. Advantages:
1. facilitates long term predictions
2. allows for the efficient prediction of throusands of compact state sequences in parallel in a single batch without having to generate images.
<!--ID: 1685719949105-->


Q: What does an experience dataset consist of in RL?
A: Sequences inputs $x_{1:T}$ (e.g., images), actions $a_{1:T}$ , rewards $r_{1:T}$, and discount factors $\gamma_{1:T}$.
<!--ID: 1685719949107-->


Q: List the model components of dreamer V2
A: (1) Image encoder, (2) Recurrent State-Space Model to learn the dynamics, (3) predictors for the image, reward and discount factor.
![[Pasted image 20230602104037.png]]
<!--ID: 1685719949109-->



Q: What is the Reccurent State-Space Model in Dreamer V2? E.g., what are its inputs and outputs?
A:  The RSSM has three components:
1. The recurrent model:
	- Inputs: previous hidden state $h_{t-1}$, previous posterior state $z_{t-1}$, and previous action $a_{t-1}$
	- Outputs: new hidden state $h_t = f_\phi(h_{t-1}, z_{t-1}, a_{t-1})$
2. The representation model: 
	- Inputs: $h_t$, the image encoding $x_t$ 
	- Outputs: A sample from posterior over the stochastic state: $z_t \sim q_\phi(z_t | h_t, x_t)$
3. The transition predictor:
	- Inputs: $h_t$
	- Outputs: A sample from the prior over the stochastic state $\hat{z}_t  \sim p_\phi(\hat{z}_t | h_t)$
The distribution over the stochastic state is a catagorical random variable with 32 catagories and 32 classes each. 
<!--ID: 1685719949111-->


Q: What are the inputs to the image predictor, reward predictor, and discount predictor in dreamer V2?
A: The concatentation of the deterministic recurrent state $h_t$ and a sample of the stochasic state $z_t$. 
<!--ID: 1685719949114-->


Q: Give four hypothesis why dreamer V2 catagorical latents outperform diagonal gaussian latents
A: hypothesis
1.  A categorical prior can perfectly fit the aggregate posterior, because a mixture of categoricals is again a categorical. In contrast, a Gaussian prior cannot match a mixture of Gaussian posteriors, which could make it difficult to predict multi-modal changes between one image and the next.
2. The level of sparsity enforced by a vector of categorical latent variables could be beneficial for generalization. Flattening the sample from the 32 categorical with 32 classes each results in a sparse binary vector of length 1024 with 32 active bits.
3. Despite common intuition, categorical variables may be easier to optimize than Gaussian variables, possibly because the straight-through gradient estimator ignores a term that would otherwise scale the gradient. This could reduce exploding and vanishing gradients.
4. Categorical variables could be a better inductive bias than unimodal continuous latent variables for modeling the non-smooth aspects of Atari games, such as when entering a new room, or when collected items or defeated enemies disappear from the image
<!--ID: 1685719949116-->


Q: Describe how we can train the logits of a catagorical distribution using a sample from that distribution?
A: Use a pass-through gradient estimator (e.g., gradients on the sample are used for the logits):
![[Pasted image 20230602110331.png]]
<!--ID: 1685719949118-->



Q: What is the loss function for the world model of dreamer V2?
A: ![[Pasted image 20230602110556.png]]
<!--ID: 1685719949121-->


Q: What is KL balancing?
A: A technique for training the prior and approximate posterior at different rates through KL divergence:![Pasted image 20230602111222.png](app://9a4aa2fc00c81112116c5393a69c73ee5862/Users/benagro/research_papers/Pasted%20image%2020230602111222.png?1685718742771)
![[Pasted image 20230602111222.png]]
<!--ID: 1685719949123-->






Q: Visualize the world model diagram for dreamer V2
A: ![[Pasted image 20230602111646.png]] 
<!--ID: 1685719949125-->


Q: Visualize the Actor-Critic learning procedure in Dreamer V2
A: ![[Pasted image 20230602111749.png]] 
<!--ID: 1685719949127-->



Q: What are the actor and critic models in dreamer V2?
A: Actor: aims to output actions that lead to states that maximize the critic output $\hat{a}_t  = p_\psi(\hat{a}_t, \hat{z}_t)$. Critic: aims to accurately estimate the sum of future rewards achieved by the actor from each imageined state: $v_\xi(\hat{z}_t) \approx E_{p_\psi, p_\phi}[\sum_{\tau > t} \hat{\gamma}^{\tau - t} \hat{r}_\tau]$. The rewards, discounts, latent states come from the rollout of the world model using actions from the actor.
![[Pasted image 20230602111749.png]] 
<!--ID: 1685719949129-->






