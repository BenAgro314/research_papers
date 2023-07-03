Title: Hidden Biases of End-to-End Driving Model
Authors: #Bernhard_Jaeger #Kashyap_Chitta #Andreas_Geiger
Tags: #CARLA
Year: 2023
Link: https://arxiv.org/pdf/2306.07957.pdf

### Notes

- Observation: IL methods on CARLA seem weirdly to not suffer from the compounding error problem. 
- These IL algorithms do not use HD maps as input, instead using target points (TP) (spaced 30 m apart on average) along the desired lane centerlines (as a route input)
- This work shows that TP conditioned models recover from the compounding error problem because they use geometric information in the TP to reset steering errors at every TP
	- *"methods learn to steer towards nearby TPs because the expert trajectory in the dataset always goes through them"*
	- The hypothesis here is that the global average pooling (GAP) decoders do not preserve spatial information for waypoint decoding
	- If you use an attention archiecture with positional encodings, then maybe this output is preserved
- This makes the implicility rely on HD map information, even tho they are otherwise map-free 
- When methods accumulate enough error to be OOD during depolyment, they learn to just turn towards the nearest TP. This can have issues if the car is from the nearest TP and OOD, e.g., cutting a turn
- demonstrates that this behavior is inherent to the decoder archiecture, and that a transformer decoder can mitigate this issue. 

- Another common aspect of SOTA methods is they use waypoint (future positions of expert driver) as output representations.
- This is an ambiguous representation, as the future velocity is multi-modal, yet the model commits to a point estimate
- This can be sometimes useful due to the continuous nature of waypoints: the vehicle can interpolate between modes
- This paper proposes an alternative that explicitly predicts the uncertainty of the network via target speed classification, and shows that interpolating between target speeds weighted by the uncertainty classification reduces collisions


### Anki

Q: What is the common problem with IL-based methods (e.g., for self-driving)
A: They suffer from the issue of compounding errors, which take the agent OOD and then it produces bad predictions
<!--ID: 1687702914732-->


Q: What is global average pooling?
A: Average each feature map. E.g., if the input is a tensor of shape (B, F, H, W), the output will be of shape (B, F, 1, 1). Usually F will be the number of classes, so you can re-shape to (B, F), run softmax on the last dimension to get class probabilities.
<!--ID: 1687702914745-->





