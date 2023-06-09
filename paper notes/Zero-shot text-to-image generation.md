Title: Zero-shot text-to-image generation
Authors: #Aditya_Ramesh, #Mikhail_Pavlov, #Gabriel_Goh, #Scott_Gray, #Chelsea_Voss, #Alec_Radford, #Mark_Chen, #Ilya_Sutskever
Tags: #OpenAI #DALL-E
Year: 2021
Link: https://arxiv.org/pdf/2102.12092.pdf


### Method

Two stage training process:
1. Train a discrete VAE (dVAE) to compress each 256x256 RGB image into a 32x32 grid of image tokens, each element of whch can assume 8192 possible values (reduces context size)
2. Concatenate up to 256 BPE-encoded text tokens with the 32x32=1024 image tokens, and train an autoregressive transformer to model the joint distribution over the text and image tokens

Joint distribution over images $x$, captions $y$ , and tokens $z$:
$$p_{\theta, \psi}(x, y, z) = p_\theta(x | y, z) p_\psi(y, z)$$

Loss is ELBO:
$$E_{z \sim q_\theta(z | x)}[\ln p_\theta(x | y, z) - \beta D_{KL} (q_\phi(y, z | x), p_\psi(y, z)) ]$$
- $p_\psi$ is learned by the transformer
- $q_\phi$ is the distribution over 32x32 image tokens generated by the **dVAE encoder** given the RGB image
- $p_\theta$ denotes the distribution over the RGB images generated by the **dVAE decoder** given image tokens

Two stage optimization:
1. Learning the visual codebook by maximizing ELBO wrt $\phi$ and $\theta$ (training the dVAE on images alone) . Use #gumbel-softmax to approximate gradients through discrete distribution 
2. Learning the prior: 
	- fix $\phi$ and $\theta$ and learn the prior distribution over the text and image tokens by maximizing the ELBO wrt $\psi$
	- transformer is decoder only. Three types of self attention masks
		- text-to-text attention uses causal mask
		 - image-to-image uses either a row, column, or convolutional mask
	 - Use a learned padding token to fill the tokens between the caption and image when the caption is shorter than 256 tokens
	  - 
 


### Anki

  
Q: What is the first stage of the two-stage training process in the Zero-shot text-to-image generation paper?
A: The first stage is to train a discrete Variational Autoencoder (dVAE) to compress each 256x256 RGB image into a 32x32 grid of image tokens, where each element can assume 8192 possible values.
<!--ID: 1684258344174-->


Q: What is the second stage of the two-stage training process in the Zero-shot text-to-image generation paper?
A: The second stage is to concatenate up to 256 BPE-encoded text tokens with the 32x32=1024 image tokens and train an autoregressive transformer to model the joint distribution over the text and image tokens.
<!--ID: 1684258344186-->


Q: What is the joint distribution over images x, captions y, and tokens z in the Zero-shot text-to-image generation paper? What models correspond to each part of the joint distribution?
A: Joint distribution over images $x$, captions $y$ , and tokens $z$:
$$p_{\theta, \psi}(x, y, z) = p_\theta(x | y, z) p_\psi(y, z)$$
$p_\theta$ is the dVAE decoder and $p_\psi$ is the transformer.
<!--ID: 1684258344188-->



Q: What is the loss function in the Zero-shot text-to-image generation paper
A: Loss a weighted version of ELBO:
$$E_{z \sim q_\theta(z | x)}[\ln p_\theta(x | y, z) - \beta D_{KL} (q_\phi(y, z | x), p_\psi(y, z)) ]$$
- $p_\psi$ is learned by the transformer
- $q_\phi$ is the distribution over 32x32 image tokens generated by the **dVAE encoder** given the RGB image
- $p_\theta$ denotes the distribution over the RGB images generated by the **dVAE decoder** given image tokens
<!--ID: 1684258344191-->
