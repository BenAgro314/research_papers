Title: Taming Transformers for High-Resolution Image Synthesis
Authors: #Patrick_Esser, #Robin_Rombach, #Bjorn_Ommer
Tags: #IWR  #CVPR
Year: 2021
Link: https://openaccess.thecvf.com/content/CVPR2021/papers/Esser_Taming_Transformers_for_High-Resolution_Image_Synthesis_CVPR_2021_paper.pdf

### Anki

Q: What do we sacrifice when naively applying transformers to images compared to CNNs? what do we gain?
A: sacrifice the inductive prior on locality of relationships (higher computational costs), gain generality
<!--ID: 1684374162860-->



Q: What is the key inside in the VQ-GAN paper?
A: use a convolutional approach to efficiently learn a codebook of context-rich visual parts and learn a model of their global compositions.
<!--ID: 1684374162866-->


Q: How are images represented in the VQ-GAN paper?
A: As sequences of tokens from a learned codebook. This is a compression of the image information into fewer tokens, so that a transformer can be used to autoregressively generate new tokens (e.g., new images). Tokens are in raster-scan order
<!--ID: 1684374162869-->


Q: How is the codebook learnt in VQ-GAN paper?
A: Learn a convolutional model consisting of an encoder $E$ and decoder $G$  and apply the VQ-VAE framework (straight-through gradient estimator), and introduce an adverserial training procedure with a patch-based discriminator D that mains to differentiate between real and reconstructed images:
$$\mathcal{L} = \mathcal{L}_{VQ} + \lambda \mathcal{L}_{GAN}$$
<!--ID: 1684374162871-->


Q: Describe training procedure of the VQ-GAN paper:
A: Training steps:
1. Train a VQ-VAE style encoder, but using a discriminator as part of the loss function (GAN)
2. Train a transformer to auto-regressively predict tokens in the image (choosing an ordering like raster-scan). The transformer can be conditioned on other inputs such as a sequence of text tokens.
![[Pasted image 20230517213020.png]]
<!--ID: 1684374183028-->
