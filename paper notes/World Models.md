Title: World Models
Authors: #David_Ha, #Jurgen_Schmidhuber
Tags: #NeurIPS 
Year: 2018
Link: https://worldmodels.github.io/


### Anki

Q: What is a *world model*? (humans)
A: "The image of the world around us, which we carry in our head, is just a model. Nobody in his head imagines all the world, government or country. He has only selected concepts, and relationships between them, and uses those to represent the real system" 
<!--ID: 1686059896034-->


Q: Describe how baseball is evidence for internal world models
A: A baseball batter has milliseconds to decide how they should swing the bat --- shorter than the time it takes for visual signals from our eyes to reach our brain. The reason we are able to hit a fastball is due to our ability to instinctively predict when and where the ball will go
<!--ID: 1686059896043-->


Q: Describe the *credit assignment problem* in RL in a sentence
A: In many RL problems, the feedback (positive or negative reward) is given at end of a sequence of steps. The credit assignment problem tackles the problem of figuring out which steps caused the resulting feedback—which steps should receive credit or blame for the final result? 
<!--ID: 1686059896046-->


Q: Why is it hard to train large networks in model-free RL?
A: The credit assignment problem
<!--ID: 1686059896049-->


Q: How do world models allow for the training of large RNNs with RL?
A: Train a large world model, and a small controller model. A small controller lets the training algorithm focus on the credit assignment problem on a small search space, while not sacrificing capacity and expressiveness via the larger world model. 
<!--ID: 1686059896051-->



Q: Visualize the rollout of a world-model RNN (Ha & Schmidhuber)
A: Diagram:
![[Pasted image 20230606093344.png]]
<!--ID: 1686059896054-->


Q: What is the **V** model in world models?
A: The vision model. It is the encoder portion of a VAE, which learns a latent code of the observations
![[Pasted image 20230606093424.png]]
<!--ID: 1686059896056-->


Q: What is the **M** model in world models?
A: An MDN-RRN Model: The role of the M model is to predict the future, to compress what the agent sees at each time frame. 
![[Pasted image 20230606093529.png]]
The RNN models $p(z_{t+1} | a_t, z_t, h_t)$ as a mixture of gaussians, where $a_t$ is the action taken at $t$, $h_t$ is the hidden state of the RNN at time $t$. 
<!--ID: 1686059896059-->


Q: What is the **C** network in world models?
A: A simple linear layer $a_t = W_c[z_t  \; h_t] +b_c$ which maps the concatentated input vector to output an action vector
<!--ID: 1686059896061-->


Q: Visualize the flow diagram of world models (Hu & Schmidhuber)
A: The $h_t$ is a compressed version of the future state, $z_t$ carries information about the current state
![[Pasted image 20230606093929.png]]
<!--ID: 1686059896063-->


Q: Why use a minimal controller design **C** in world models?
A: It's low parameter count allows for unconventional training methods such as evolution. It also allows for tackling the credit assignment problem more easily.
<!--ID: 1686059896066-->


Q: Four step procedure for training a world-model RL algorithm of the OpenAI car gym env
A: steps:
1. collect 10000 rollouts with random policy
2. train VAE (V) to encode each frame into latent vector $z$
3. train MDN-RNN (M) to model $p(z_{t+1} | a_t, z_t, h_t)$
4. Evolve controller (C) to maximize the expected cumulative reward of a rollout
<!--ID: 1686059896068-->


Q: What is the procedure for training a world model RL algorithm from its own dreams?
A: Steps
1. Colect rollouts from a random policy
2. train a VAE (V) to encode each frame into a latent vector $z$ and use $V$ to convert the images collected from (1) into the latent space representation
3. Train MDN-RNN (M) to model $p(z_{t+1}, d_{t+1} | a_t, z_t, h_t)$, where $d_{t+1}$ indicates whether or not the current game is done
4. evolve controller (C) to maximize the expected survival time inside the virtual env
5. Use a learned policy from (4) on actual Gym environment
<!--ID: 1686059896071-->


Q: Why do we want to tune the temperature parameter of the MDN-RNN when training in dreams (world models)?
A: We want the rollout to be sufficiently stochastic and multi-modal so that the controller can't learn to cheat using the simplifications made by the imperfect world model 
<!--ID: 1686059896073-->


Q: Describe why an iterative training procedure is needed if training with world models
A: The initial random policy may be too bad to explore the whole state space (e.g., inverted pendulum)
<!--ID: 1686059896076-->


Q: Describe the iterative training procedure with world models
A: Steps
1. Initialize $M$ and $C$ with random model parameters
2. Rollout to actual environment $N$ times. Agent may learn during rollouts. Save actions $a_t$ and obesrvations $x_t$ during rollouts to storage device.
3. train $M$ to model $p(x_{t+1}, r_{t+1}, a_{t+1}, d_{t+1} | x_t, a_t, h_t)$ and train $C$ to optimize expected rewards inside of $M$
4. Go back to (2) if task is not completed.
<!--ID: 1686059896079-->

Q: What are the advantages (3) of training an agent to perform tasks entirely inside of its simulated latent space world (world models)?
A: Practical benefits:
1. Less wasted computational resources (no rendering of image frames, or computation of useless physics)
2. Take advantage of deep learning frameworks to accelerate our world-model simulations using GPUs in a distributed environment.
3. World model is implemented as a fully differentiable recurrent computation graph, so we may be able to train our agents in the dream directly using the backpropagation algorithm to fine-tune its policy to maximize an objective function
<!--ID: 1686147436419-->


