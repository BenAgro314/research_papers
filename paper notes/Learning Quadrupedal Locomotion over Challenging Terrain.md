Title: Learning Quadrupedal Locomotion over Challenging Terrain
Authors: #Joonho_Lee  #Jemin_Hwangbo   #Lorenz_Wellhausen #Vladlen_Koltun   #Marco_Hutter
Tags: #ETH 
Year: 2020
Link: https://arxiv.org/pdf/2010.11251.pdf


### Notes




### Anki


Q: What are the three important ideas that "Learning Quadrupedal Locomotion over Challenging Terrain" found was necessary for robust zero-shot four legged locomotion with only propreioceptive sensors?
A: Three ideas:
1. A temporal convolutional archiecture that takes into account a history of propreioceptive sensor data (instead of an MLP that only considers the current robot state)
2. Privileged learning: straight RL from proprioceptive data is hard (sparse rewards). Instead, a privilieged policy was trained with ground truth information from the simulator. Then this privilged policy is distilled into the actual policy via imitation learning on the experts actions and latent states.
3. Automated curriculum: synthesizes terrains in simulation such that it is of moderate difficulty for the policy.
<!--ID: 1688162525270-->



