Title: Categorical reparameterization with gumbel-softmax
Authors: #Eric_Jang #Shixiang_Gu  #Ben_Poole
Tags: #Google_Brain #ICLR 
Year: 2017
Link: https://arxiv.org/pdf/1611.01144.pdf

## Terminology

- #simplex: an n-dimensional simplex isa geometric shape that is the generalization of the triangle to n dimensions
	- 0D simplex is a point
	- 1D simplex is a line
	- 2D simplex is a triangle
	- 3D simplex is a tetrahedron
 - In the context of probability theory, a simplex is the set of all n-dimensional vectors whose elements are non-negative and sum to 1.
	 - e.g., 2D simplex: set off all points $(p, q)$ such that $p \ge 0$, $q \ge 0$ and $p + q = 1$.
		  - this can be visualized as a line segment from $(1, 0)$ to $(0, 1)$ in the 2D plane
	


## Motivation

- we like discrete distributions for:
	- interpretability,
	- computational efficiency 
- We can't backpropogate through samples from a categorical distribution (e.g., like in #VQ-VAE's)


### Contributions

- This paper proposes an efficient gradient estimator to replaces the non-differentiable sample from a categorical distribution with a differentiable sample from a novel #gumbel-softmax distribution
- #gumbel-softmax , a continuous distribution on the simplex that can approximate categorical samples, and whose parameter gradients can be easily computed via the reparameterization trick


### Anki

Q: What is the Gumbel-Max trick?
A: A method for sampling from a categorical distribution:
$$z = \text{one\_hot}\left(\text{argmax}_i [g_i + \log \pi_i])\right)$$
where $g_i$'s are drawn from the Gumbel distribution, and $\pi_i$ are the class probabilities.
<!--ID: 1683650871138-->


Q: What is the Gumbel-Softmax trick? How is it motivated by the Gumbel max trick?
A: The Gumbel-Max trick involves sampling from a categorical distrubtion $\pi_i$ with draws from a gumbel distribution $g_i$:
$$z = \text{one\_hot}\left(\text{argmax}_i [g_i + \log \pi_i])\right)$$
The Gubmel-Softmax makes the differentiable by replacing the $\text{one\_hot} (\text{argmax}())$  with a soft_max:
$$y_i = \frac{\exp((\log(\pi_i) + g_i)/\tau)}{\sum_{j=1}^k \exp((\log(\pi_j) + g_j)/\tau)}, \quad \forall i = 1, \dots, k$$
which makes it differentiable, and suitable for use when training a neural network with backprop
<!--ID: 1683650871147-->



Q: How does the temperature parameter $\tau$ affect the gumbel-softmax?
A: As $\tau \to 0$, the gumbel-softmax distrubtion approaches the true one-hot categorical, as $\tau \to \infty$, the gumbel-softmax samples are no longer one-hot and become uniform. 
<!--ID: 1683650871150-->


Q: Why do we anneal $\tau$ when training with a gumbel-softmax
A: Start with large $\tau$ (not very accurate, but low variance gradient estimate), and decrease $\tau$ to a small but non-zero value (accurate, but higher variance gradient estimate)
<!--ID: 1683650871153-->



Q: Visualize the gumbel-softmax distrubtion for different temperature parameters $\tau = 0, 1/2, 1, 2$ on a 2-dimensionl simplex.
A: ![[Pasted image 20230509124737.png]]
<!--ID: 1683650871155-->
