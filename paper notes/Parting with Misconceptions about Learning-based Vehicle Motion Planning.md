Title: Parting with Misconceptions about Learning-based Vehicle Motion Planning
Authors: #Daniel_Dauner #Marcel_Hallgarten #Andreas_Geiger  #Kashyap_Chitta 
Tags: #nuPlan
Year: 2023
Link: https://arxiv.org/pdf/2306.07962.pdf


### Notes

#### Findings from NuPlan

1. Open and closed loop evaluation are mis-aligned
	- a negative correlation exists between open-loop evaluation of ego-forecasting and closed loop driving performance
1. Rule based planning generalizes
	- An established rule-based planning baseline from 20 years ago surpassess all  SOTA learning based methods in terms of closed loop evaluation on nuPlan
	- This contradicts the prevailing claim that rule-based planners don't generalize
1. A centerline is all you need for ego-forecasting
	- This paper implements a naive learning-based baseline which does not incorporate any input from other agents in the scene and merely extrapolates the ego state conditioned on a centerline represenation of the desired route
1. Long-horizon prediction adds little value
	- This paper proposes novel rule-based and learned planners that achieve SOTA results on open-loop and closed-loop evaluation metrics.
	- Also introduces a learning-based planner that adds a corrective offset to the outputs of a rule-based planner. This hybrid strategy greatly improves performance in open-loop, but not closed loop.

### Anki

Q: What are the four takeaways from the paper "parting with misconceptions about learning based vehicle motion planning", from Andreas Geiger?
A: Takeaways:
1. Open an closed loop evaluation are mis-aligned
2. Rule based planning generalizes
3. A center-line is all you need for ego-forecasting
4. Long-horizon prediction adds little value
<!--ID: 1687713074369-->


