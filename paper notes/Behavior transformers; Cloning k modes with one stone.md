Title: Behavior transformers; Cloning k modes with one stone
Authors: #Nur_Muhammad_Shafiullah #Zichen_Jeff_Cui #Ariuntuya_Altanzaya #Lerrel_Pinto
Tags: #NeurIPS 
Year: 2022
Link: https://arxiv.org/pdf/2206.11251.pdf

### Anki

Q: Give a basic description of behaviour transformers (BeT) (2022 NeurIPS). Visualize the model diagram
A: Dataset consists of pairs of observations and actions. The continuous actions are clustered into k bins, and a transformer is used to learn to output a categorical distribution over the bins given a sequence of observations. A header on the transformer decoder also regresses an action offset from the bin center (so it is viable in continuous space). ![[Pasted image 20230512200412.png]]
<!--ID: 1683936264445-->


Q: Describe how behaviour transformers (BeT) (2022 NeurIPS) might be better for model multi-modal imitation learning than observation-conditioned l2 regression 
$$\theta^* = \text{argmax}_\theta \prod_t \mathbb{P}(a_t |o_t; \theta) \rightarrow \sum_t ||a_t - \pi(o_t; \theta)||^2$$
A: This l2 regression assumes that the data distribution is restricted to unimodal isotropic gaussians. BeTs are essentially learning a mixture of gaussians (although they don't predict mode centers), which allows for multimodality. Further, they allow $a_t$ to be conditioned on a sequnence of past observations $o_t, o_{t-1}, \dots, o_{t-h+1}$ (relaxing the Markovian assumption)
<!--ID: 1683936264451-->
