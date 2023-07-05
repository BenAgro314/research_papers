Title: Learning From All Vehicles
Authors: #Dian_Chen #Philipp_Krahenbuhl 
Tags: #UT_Austin  #CVPR 
Year: 2022
Link: https://arxiv.org/pdf/2203.11934.pdf


### Notes


### Anki

Q: How does "Learning from all Vehicles" work?
A: At training time, a pre-trained backbone (on semantic segmentation and detection tasks) is used to produce a BEV feature map, features are extracted (ROIAlign) from labelled actor + ego locations, and those features are used to predict future trajectories. Then at inference time, the ego location is cropped from the BEV feature map to get a motion plan. 
Notes:
- The motion planner is conditioned on a high-level plan, which is infered by the model for non-ego vehicles (e.g., the plans are produced for all commands, with a winner-take-all loss).
- The motion-planner is trained via distillation. First a pred-only model is trained from ground truth detections, and then it is distilled into a pnp model
<!--ID: 1688504568273-->



### Questions / Future Work


- Dynamics / feasible waypoints differ for different vehicles 
- You might not want to learn from "all" drivers, only those that are actually good at driving
	- Maybe this is key tho. If every driver was an expert, there would be less examples of navigating safety critical scenarios
- Does distillation help only for go prediction, or does it also help for predictions of the other actors?