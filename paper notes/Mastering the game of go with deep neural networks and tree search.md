Title: Mastering the game of go with deep neural networks and tree search
Authors: #David_Silver, #Aja_Huang, #Christopher_J_Maddison, #Arthur_Guez, #Laurent_Sifre, #George_van_den_Driessche, #Julian_Schrittwieser, #Ioannis_Antonoglou, #Veda_Panneershelvam, #Marc_Lanctot, #Sander_Dieleman, #Dominik_Grewe, #John_Nham, #Nal_Kalchbrenner, #Ilya_Sutskever, #Timothy_Lillicrap, #Madeleine_Leach, #Koray_Kavukcuoglu, #Thore_Graepel, #Demis_Hassabis
Tags:  #DeepMind  #Nature #MCTS #RL
Year: 2016
Link: https://www.rose-hulman.edu/class/cs/csse413/schedule/day16/MasteringTheGameofGo.pdf


### Notes 

What are the four networks used in alpha go? 
Networks:
1. Supervised learning policy network $p_\sigma$ : A CNN trained to output a probability distribution over  the most likely moves. Trained on human data.
2. Rollout policy $p_\theta$: A smaller version of $p_\sigma$ with much faster inference.
3. RL policy network $p_\rho$ : identical to $p_\sigma$ and with weights initialized to $\rho = \sigma$, but trained via reinforment learning with self-play. This moves from trying to mimic humans to trying to win games
4. Value network $v_{\theta}(s)$: trained to evaluate the value of positions on state outcome pairs $(s, z)$  via minimizing MSE. Trained on data from self-play.

The following networks are used in AlphaGo
1. Supervised learning policy network $p_\sigma$ : A CNN trained to output a probability distribution over  the most likely moves. Trained on human data.
2. Rollout policy $p_\theta$: A smaller version of $p_\sigma$ with much faster inference.
3. RL policy network $p_\rho$ : identical to $p_\sigma$ and with weights initialized to $\rho = \sigma$, but trained via reinforment learning with self-play. This moves from trying to mimic humans to trying to win games
4. Value network $v_{\theta}(s)$: trained to evaluate the value of positions on state outcome pairs $(s, z)$  via minimizing MSE.
how are they used?
Actions are selected via: $a_t = \text{argmax}_{a} (Q(s_t, a) + u(s_t, a))$, where
$$u(s, a) = \frac{p_\sigma(a | s)}{1 + N(s, a)}$$
where $N(s, a)$ is the visit count of the search tree edge $(s, a)$, and the action value $Q(s, a)$ is:
$$Q(s, a) = \frac{1}{N(s, a)}\sum_{i=1}^n 1(s, a, i) (0.5 * v_\theta(s_L^i + 0.5 z_L)$$
where $s_L^i$ is the leaf node form the ith simulation, and $1(s, a, i)$ indicates whether an edge $(s, a)$ was traversed durning the $i^{th}$ simulation, and $z_L$ is the outcome of random rollout of the game played with $p_\theta$. 


What network is used for evaluating actions in AlphaGo during gameplay 
The network trained via supervised learning on human data (not reinforcement learning). This is because the humans select a more diverse beam of promising moves. 

### Anki

Q: What are the four networks in Alpha Go? (Just name them)
A: Rollout policy, SL policy network, RL policy network, Value network
<!--ID: 1684543385498-->


Q: What is the SL policy network in Alpha GO?
A: $p_\sigma(a | s)$ , trained to predicted most likely action from board state, supervised learning on human data. A deep convolutional network
<!--ID: 1684543385505-->


Q: What is the rollout policy in Alpha GO?
A:  A much shallower version of the SL policy network
<!--ID: 1684543385507-->


Q: What is the RL policy network in Alpha GO? How is it trained?
A: $p_\rho(a | s)$, initialized to the weights of the SL policy network. Trained via self play against other (previous) versions of itself. The winning network has its weights updated towards its gradient, and the losing network away (policy gradient method)
<!--ID: 1684543385510-->


Q: What is the value network in Alpha GO? What is it trained on?
A: $v_\theta(s)$ evaluates how good a board state is. Trained to minimize MSE on tuples $(a, z)$ , $z = \pm 1$ depending on the output of the game. It is trained on a combination of data from human games and data from self-play.
<!--ID: 1684543385512-->


Q: What network is used for evaluating actions in AlphaGo during gameplay 
A: The network trained via supervised learning on human data (not reinforcement learning). This is because the humans select a more diverse beam of promising moves. 
<!--ID: 1684543385515-->


Q: What is the rule for selecting actions in AlphaGO from state $s_t$?
A: $a_t = \text{argmax}_a( Q(s_t, a) + u(s_t, a))$, where $Q(s_t, a)$ is the action value computed via simulation (with value networks and rollout network), and $u(s_t, a)$ is a bonus proportional to the prior probability given by the SL policy network, decaying with repeated visits (to encourage exploration)
<!--ID: 1684543385518-->
