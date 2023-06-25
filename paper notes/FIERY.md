Title: FIERY
Authors: #Anthony_Hu, #Zak_Murez, #Nikhil_Mohan, #Sofía_Dudas, #Jeffrey_Hawke, #Vijay_Badrinarayanan, #Roberto_Cipolla, #Alex_Kendall
Tags: #wayve
Year: 2021
Link: https://arxiv.org/abs/2104.10490


### Anki

Q: Describe each step in the FIERY archiectecture
![[Pasted image 20230603192649.png]]
A: Steps
1. At each past timestep, we lift camera inputs to 3D by predicting a depth probability distribution over pixels and using known camera intrinsics and extrinsics
2. These features are projected to BEV. Using based ego motion, we transform the featuers to the present reference time with a spatial transformer
3. A 3D conv net temporal model learns a spatio-temporal state 
4. We parameterize two probability distributions: the present and future dist. The present dist is conditioned on the current state, while the future dist is conditioned on the current state and future labels.
5. We sample a latent code from the future distribution during training, and from the present distribution during inference. The current state and latent code are the inputs to the future prediction model that recursibely predicts future states
6. The states are decoded into future instance segmentation and future motion in BEV
<!--ID: 1685835622376-->


Q: What are the outputs of the FIERY model?
A: Outputs
1. heatmap of instance centereness (probability of instance center)
2. segmentation
3. vector field indicating the direction to the instance center
4. future ego motion
5. Instance segmentation, obtained by NMS
![[Screen Shot 2023-06-03 at 7.34.52 PM.png]]
<!--ID: 1685835622383-->


Q: How can FIERY obtain instance segmentation?
A: (i) find instance centeres via NMS (ii) assign neighbouring pixels the ID of the nearest center using the offset field (iii) warp instance center's using the future flow field (these are matched against future detections using hungarian matching)
<!--ID: 1685835622386-->


