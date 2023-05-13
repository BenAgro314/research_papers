Title: Neural discrete representation learning
Authors: #Aaron_van_den_Oord, #Oriol_Vinyals, #Koray_Kavukcuoglu
Tags: #DeepMind  #NeurIPS #VAE #VQ-VAE
Year: 2017
Link: https://arxiv.org/pdf/1711.00937.pdf

## VAEs

- $q(z|x)$: posterior over latent space, usually diagonal gaussian (learnt)
- $p(z)$: prior over latent space, usually diagonal gaussian (assumed)
- $p(x|z)$: decoder distribution over data space (learnt)

## Method

### Contributions

- Avoids issues of posterior collapse (where $p(x|z)$ learns to not depend on $z$ and output a nearly generic output)
- Can generate high-quality images, videos, and speech

### Summary
- $q(z  | x)$ and $p(z)$ are categorical distributions over discrete latent space
	- samples drawn from these distributions index an embedding table
- Latent embedding space: $e \in R^{K \times D}$, where  $K$ is the size of the discrete latent space (K-way categorical), and $D$ is the dimensionality of each latent embedding vector.


### Steps
1. Model takes an input $x$, passed through an encoder to produce output $z_e(x)$. 
2. Discrete latent variables $z$ are calculated by a nearest neighbour look-up using the shared embedding space $e$ 
4. $q(z|x)$ defines a categorical distribution, which is deterministic based on the embedding space:   $$q(z = k | x) = \begin{cases} 1 & \text{for } k = \text{argmin}_j||z_e(x) - e_j||_2 \\ 0 & \text{otherwise} \end{cases}$$
5. The representation $z_e(x)$ is passed through the discretization bottleneck followed by mapping onto the nearest element of embedding $e$: $$z_q(x) = e_k, \text{ where } k = \text{argmin}_j || z_e(x) - e_j ||_2$$

![[Pasted image 20230508125504.png]]


### Learning

- To approximate gradient, we copy the gradients from decoder input $z_q(x)$ to $z_e(x)$.
- Use a uniform prior $p(z)$ . The term $KL(q(z|x) || p(z))$  in the $ELBO$ is a constant (Because $q(z | x)$) is just a $1$ or $0$), so it can be ignored for training. 
- Loss function: $$L = \log p(x | z_q(x)) + ||sg[z_e(x)] - e||_2^2 + \beta || z_e(x) - sg[e] ||_2^2$$
	- $sg$ stands for stop gradient
	- The first term is the log-likelihood (reconstruction loss, used to train decoder and encoder)
	- Second term forces the embeddings to be near the decoder outputs (trains embeddings only). This is called Vector Quantization (VQ, #VQ).
	- Third term forces the encoder to commit to an embedding and keep its output from growing (since the volume of the embedding space is dimensionless, it can grow arbitrarily if the embeddings $e_i$ do not train as fast as the encoder parameters)
	- Usually set $\beta = 0.25$

### Prior

- $p(z)$ is a categorical distribution
- During training $p(z)$ is kept constant and uniform
- After training, learn a categorical prior $p(z)$ like pixelCNN over the latents


### Anki

Q:  How is the discrete latent variable $z$ calculated in the VQ-VAE method?
A: The discrete latent variables $z$ are calculated by a nearest neighbor look-up using the shared embedding space $e$. The model takes an input $x$, passed through an encoder to produce output $z_e(x)$. Then, $z_q(x)$ is obtained by mapping $z_e(x)$ onto the nearest element of embedding $e$.
<!--ID: 1683642901407-->


Q: How is the gradient approximated in the VQ-VAE learning process?
A: To approximate the gradient in the VQ-VAE learning process, the gradients from the decoder input $z_q(x)$ are copied to $z_e(x)$.
<!--ID: 1683642901418-->


Q: What is the loss function used in the VQ-VAE method and what does each term represent?
A: Loss function: $$L = \log p(x | z_q(x)) + ||sg[z_e(x)] - e||_2^2 + \beta || z_e(x) - sg[e] ||_2^2$$
	- $sg$ stands for stop gradient
	- The first term is the log-likelihood (reconstruction loss, used to train decoder and encoder)
	- Second term forces the embeddings to be near the decoder outputs (trains embeddings only). This is called Vector Quantization (VQ, #VQ).
	- Third term forces the encoder to commit to an embedding and keep its output from growing (since the volume of the embedding space is dimensionless, it can grow arbitrarily if the embeddings $e_i$ do not train as fast as the encoder parameters)
	- Usually set $\beta = 0.25$
<!--ID: 1683642901421-->


Q:  Draw a diagram of the VQ-VAE architecture
A: ![[Pasted image 20230508125504.png]]
<!--ID: 1683642901423-->


Q: What is the prior used in VQ-VAE during training and inference
A: Prior:
- $p(z)$ is a categorical distribution
- During training $p(z)$ is kept constant and uniform (this means the $KL(q(z | x) || p(z))$  term in elbo turns into a constant)
- After training, learn a categorical prior $p(z)$ (like pixelCNN) over the latents
<!--ID: 1683642942585-->


