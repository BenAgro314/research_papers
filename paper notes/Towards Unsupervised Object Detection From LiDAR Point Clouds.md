Title: Towards Unsupervised Object Detection From LiDAR Point Clouds
Authors:   #Lunjun_Zhang, #Anqi_Joyce_Yang, #Yuwen_Xiong, #Sergio_Casas, #Bin_Yang, #Mengye_Ren, #Raquel_Urtasun
Tags:  #Waabi #UofT #AV #OYSTER
Year: 2022
Link: https://openaccess.thecvf.com/content/CVPR2023/papers/Zhang_Towards_Unsupervised_Object_Detection_From_LiDAR_Point_Clouds_CVPR_2023_paper.pdf


### Notes



### Anki

Q: What are object-centric models in deep learning? 
A: The image / input is composed of K different objects. The model has an encoder which decomposes the image into parts, and a decoder (renderer) that reconstructs the objects from the parts. Minimize a reconstruction loss. The inductive priors force the encoder to learn to recognize objects.
<!--ID: 1684601413557-->


Q: What is open-set detection?
A: A weakly supervised setting where the system is given labels on certain known objects but is expected to discover the unknown objects on its own.
<!--ID: 1684601413564-->


Q: What are the two phases of training for the OYSTER paper?
A: (1) initial bootstrapping, where objects in the near range are detected via clustering (because point clouds are dense) to create pseudo labels. Then a CNN can be trained to detect those labels (with beam dropout), and (2) self-improvement phase where, an offline tracker is used to find long tracks and improve the labels for training a new detector on the updated pseudo labels.
![[Pasted image 20230520124850.png]]
<!--ID: 1684601413566-->


