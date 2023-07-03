Title: Learning to drive from a world on rails
Authors: #Dian_Chen #Vladlen_Koltun #Philipp_Krahenbuhl 
Tags: #UT_Austin #Intel #ICCV
Year:  2021
Link: https://dotchen.github.io/world_on_rails/

### Notes



### Anki

Q: What is offline reinforcement learning?
A: Another name for *learned model-based reinforcement learning*. Basically instead of being given a transition function (model of the world), you have to learn it from logs of data
<!--ID: 1688393822652-->


Q: What is hard about offline reinformcement learning?
A: It is difficult to teach a DNN that can accurately simulate the world internally (e.g., Drive GPT)
<!--ID: 1688393822655-->


Q: What is the world-on-rails assumption?
A: Posits that the agent and the environment are independent, and that the environment does not react to the agent
<!--ID: 1688393822657-->


Q: How does "world on rails" factorize the transition function $L_{t+1} = \mathcal{T}(L_t, a_t)$, where $L_t$ is the state of the world and ego vehicle, $a_t$ is the action the ego vehicle takes, and $L_{t+1}$ is the new state of the ego vehicle.
A: The state is factorized like $L_t = (L^{ego}_t, L^{world}_t)$, and we make the assumption that the world does not react to the ego: $L^{ego}_{t+1} = \mathcal{T}^{ego}(L^{ego}_t, L^{world}_t, a_t)$ and $L^{world}_{t+1} = \mathcal{T}(L^{world}_t)$  (so the first world state determines all future world states).
<!--ID: 1688393822660-->


Q: Describe how the optimal Bellman equation $V(L_t) = \max_a (\gamma V(\mathcal{T}(L_t, a)) + r(L_t, a))$ is factorized in the "world on rails" assumption?
A: $V(L^{ego}_t, \hat{L}^{world}_t) = \max_a (r(L^{ego}_t, \hat{L}^{world}_t, a_t)) + \gamma V(\mathcal{T}^{ego}(L^{ego}_t, \hat{L}^{world}_t,a), \hat{L}^{world}_{t+1})))$, the hats denote states from a log. Because the world states are strictly ordered in time, and are deterministic from the logs (based on the world-on-rails assumption), we can simplify this action-value function to $\hat{V}_t(L^{ego}_t) = \max_a(r(L^{ego}_t, \hat{L}^{world}_t, a_t) + \gamma \hat{V}_{t+1}(\mathcal{T}^{ego}(L^{ego}_t, a)))$. Hence, we can evaluate $\hat{V}_t$ on a volume discretized by ego states soely from a log, and then use backwards iteration to compute the action value function $\hat{Q}_t(L^{ego}_t, a_t) =  r(L^{ego}_t, \hat{L}^{world}_t, a_t) + \gamma \hat{V}_{t+1}(\mathcal{T}^{ego}(L^{ego}_t, a))$
<!--ID: 1688393822662-->


Q: Given the action value function $\hat{Q}_t(L^{ego}_t, a_t) =  r(L^{ego}_t, \hat{L}^{world}_t, a_t) + \gamma \hat{V}_{t+1}(\mathcal{T}^{ego}(L^{ego}_t, a))$ computed from the world-on-rails assumption, how is a visiomotor reactive policy learned?
A: The policy $\pi(a_t | \text{sensor data from log at time t})$ is trained to maximize the expected return actors all logs in the dataset (distillation of the privileged action value function):
	$$E_{\hat{L}^{world}_t, L^{ego}_t, \hat{I}_t} [\sum_a \pi(a | \hat{I}_t)\hat{Q}_t(L^{ego}_t, a) + \alpha H(\pi(\cdot | \hat{I}_t))]$$
<!--ID: 1688393822664-->


Q: What is being distilled in the world-on-rails paper (main idea)
A: The world on rails paper uses privilged information (privileged simulator state) to infer a (tabluar) action value function, and then distills this action value function into a non-privileged visuomotor policy.
<!--ID: 1688393822666-->


### Questions

 - I wonder how Dreamer V2 would do on CARLA?
	 - I guess this is just like MILE, which had SOTA art the time (LAV > WAR > LBC)

- Do we have a sense of how limiting the world-on-rails assumption is? Does it cap performance?

- 






