Title: A Survey of Model Compression and Acceleration for Deep Neural Networks
Authors: #Yu_Cheng, #Duo_Wang, #Pan_Zhou, #Tao_Zhang
Tags:  #Microsoft
Year: 2020
Link: https://arxiv.org/pdf/1710.09282.pdf

### Notes

Four categories of techniques:
1. parameter pruning and quantization
	- explore the redundancy in model parameters and try to remove the redundant and uncritical ones
	 - Can be extracted from pre-trained models or trained from scratch
2. low-rank factorization
	- Use matrix/tensor decomposition to estimate hte informative parameters of DNNs
	 - Can be extracted from pre-trained models or trained from scratch
3. transferred/compact convolutional filters
	- Design special structural convolutional filters to reduce the parameter space and save storage / computation
	- Only for convolutional layers
	 - Must be trained from scratch
4. knowledge distillation
	- Learn a distilled model and tran a more compact neural network to reproduce the  output of a larger neural network
	 - Must be trained from scratch
  
#### Parameter pruning and quantization

##### Quantization and Binarization

- reduce number of bits required to represent each weight 
- e.g., 8 bit or 16 bit precision
- Binarization: 1 bit representation of each weight. Directly learned during training. 
- Networks trained with backprop could be resilient to specific weight distortions like binary weights

##### Network pruning

- Reduced the number of connections based on the hessian of the loss function (see [[Optimal Brain Damage]])
- Reduce redundant weights
	- data free pruning method
	- or, use a low-cost hash function to group weights into hash buckets for parameter sharing
	- Deep compression methods can also remove redundant connections and quantize the weights, then use huffman coding to encode the quantized weights. 
	- Simple regularization: soft-weight sharing.
- growing interest in training compact DNNS with sparsity constraints, that appear in the optimization problem as $l_1$ or $l_2$ regularizers.
- **Note**: network pruning is usually able to reduce model size but not improve the efficiency (training or inference time)

### Anki

