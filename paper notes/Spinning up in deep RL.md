Title: Spinning up in deep RL
Authors: #Josh_Achiam #Pieter_Abbeel 
Tags: #OpenAI 
Year: 2018
Link: https://spinningup.openai.com/en/latest/user/introduction.html

## Part 1: Key Concepts in RL

Q: Define the following terms in RL:
- agent
- environment
- reward
- return
A: The environment is the world that the agent lives in and interacts with. The agent gets obeservations from the environment, and a reward signal (a number that indicates how good or bad it is doing). The cumulative reward is the return.
<!--ID: 1686402049751-->


Q: What is the difference between a state $s$ and an observation $o$ in RL
A: the state has all the information about the world, while the observation is a partial description of the state.
<!--ID: 1686402049758-->


Q: what is a policy in RL?
A: A rule used by an agent to decide what actions to take. It can be deterministic ($a_t = \mu(s_t)$) or stochastic $a_t \sim \pi(\cdot | s_t)$ . The policy is essentially the agents brain.
<!--ID: 1686402049761-->


Q: Two most common kinds of stochastic policies in deep RL
A: Categorical (discrete action spaces) and diagonal gaussian policies (continuous action spaces)
<!--ID: 1686402049763-->


Q: Two important computations that are centrally important for using and training stochastic policies
A: (1) sampling actions from the policy, (2) computing log likelihoods of particular actions
<!--ID: 1686402049765-->


Q: How to represent the covariance matrix of a diagonal gaussian?
A: As a vector of $\alpha_i = \log \sigma_i$ , allowing the parameters $\alpha$ to take on any real value while $\sigma$ is still a valid covariance
<!--ID: 1686402049768-->


Q: What are trajectories in RL? What governs the dynamics of the trajectories?
A:  A trajectory $\tau$ is a sequence of states and actions in the world $\tau = (s_0, a_0, s_1, a_1, \dots)$. The first state is randomly sampled from the start-state distribution $s_0 \sim \rho_0(\cdot)$ . State transitions can be deterministic ($s_{t+1} = f(s_t, a_t)$) or stochastic ($s_{t+1} \sim P(\cdot | s_t, a_t)$). Actions come from an agent according to its policy.
<!--ID: 1686402049770-->


Q: What is the reward function in RL? 
A: $r_t = R(s_t, a_t, s_{t+1})$
<!--ID: 1686402049772-->


Q: Give the equation for finite-horizon undiscounted return
A: $R(\tau) = \sum_{t = 0}^T r_t$
<!--ID: 1686402049774-->


Q: Give the equation for infinite-horizon discounted return
A: $R(\tau) = \sum_{t=0}^T \gamma^t r_t$, where $\gamma \in (0, 1)$
<!--ID: 1686402049776-->


Q: Why is the RL problem?
A: The goal is to select a policy which maximizes *expected return* when the agent acts according to it: $\pi^* = \text{argmax}_\pi J(\pi)$
<!--ID: 1686402049779-->


Q: Write the probability of a $T$-step trajectory in RL
A: $P(\tau | \pi) = \rho_0(s_0) \prod_{t=0}^{T-1} P(s_{t+1} | s_t, a_t) \pi(a_t | s_t)$
<!--ID: 1686402049781-->


Q: Give the expected return of a policy $J(\pi)$ in RL
A: $J(\pi) = \int_\tau P(\tau | \pi) R(\tau) = E_{\tau \sim \pi} [R(\tau)]$, where $\tau$ is the trajectory
<!--ID: 1686402049783-->



Q: What are value functions in RL?
A: Value functions are the expected return if you start in a state or state-action pair, and then act according to a particular policy forever.
<!--ID: 1686402049785-->


Q: What is an on-policy value function?
A: Gives the expected return if you statrt in state $s$ an always act according to policy $\pi$: $V^\pi(s) = E_{\tau \sin \pi} [R(\tau) | s_0 = s]$
<!--ID: 1686402049787-->


Q: What is an on-policy action-value function?
A: Gives the expected return if you start in state $s$, take an arbirary action $a$ (which may not come from the policy), and then forever act according to policy $\pi$:
$Q^\pi(s, a) = E_{\tau \sim \pi} [R(\tau) |  s_0 = s, a_0 = a]$
<!--ID: 1686402049789-->


Q: What is the optimal value function (RL)
A: give the expected return if you start in state $s$ and always act according to the optimal policy in the environment $V^\star(s) = \max_\pi E_{\tau \sim \pi} [R(\tau) | s_0 = s]$
<!--ID: 1686402049791-->


Q: What is the optimal action-value function (RL)
A: Give the expecte return if you start in state $s$, take arbirary action $a$, and then forever after act according to the optimal policy in the environment: $Q^\star (s, a) = \max_{\pi} E_{\tau \sim \pi} [R(\tau) | s_0 = s, a_0 = a]$
<!--ID: 1686402049794-->


Q: Connection between value function and action-value function
A: $V^\pi(s) = E_{a \sim \pi} [Q^\pi (s, a)]$ and $V^\star (s) = \max_a Q^\star(s, a)$
<!--ID: 1686402049796-->


Q: What is the optimal policy as a function of the optimal action-value function $Q^*(s, a)$?
A: $a^*(s) = \text{argmax}_a Q^*(s, a)$
<!--ID: 1686402049798-->


Q: Write bellman's question for the on-policy value functions
A: $V^\pi(s) = E_{a \sim \pi, s' \sim P} [r(s, a) + \gamma V^\pi(s')]$ and $Q^\pi(s,a) = E_{s' \sim P}[r(s, a) + \gamma E_{a' \sim \pi}[Q^\pi(s', a)]]$. These equations say that the value of your starting point is the reward you expect to get from being there, plus the value of wherever you land next. $s' \sim P$ indicates the next state is sampeld form the environment transition rules, and $a \sim \pi$ is shorthand for $a \sim \pi(\cdot | s)$ and $a' \sim \pi$ is shorthand for $a' \sim \pi(\cdot | s')$.
<!--ID: 1686402049800-->


Q: The Bellman optimal value functions are
A: $V^*(s) = \max_a E_{s' \sim P} [r(s, a) + \gamma V^\star(s')]$ and $Q^*(s, a) = E_{s' \sim P} [r(s, a) + \gamma \max_{a'} Q^*(s', a')]$
<!--ID: 1686402049802-->


Q: What is the "Bellman Backup"?
A: The bellman backup for a state or state-action pair is the right-hand side of the Bellman equation: the reward plus the next value
<!--ID: 1686402049804-->


Q: What is the advantage function in RL?
A: It describes how much better an action is than the others on average:
$A^\pi(s, a) = Q^\pi(s, a) - V^\pi(s)$
<!--ID: 1686402049806-->



## Part 2: Key Concepts and Terminology

Q: Draw the taxonomy of RL algorithms
A: ![[Pasted image 20230613221604.png]] 
<!--ID: 1686744089365-->


Q: Describe the difference between model-free and model-based RL
A: Model-based: agent has access to a model of the environment (given or learnt), which can then be used to plan ahead, and increases sample-efficiency. Model-free: agent does not have access to a model of the environment.
<!--ID: 1686744089379-->


Q: In model-free RL, what are the two catagories of things that may be learnt?
A: Policies (policy optimization) or action-value functions (Q learning)
<!--ID: 1686744089383-->


Q: What is policy optimization (model-free RL)?
A: Methods in this family represent a policy explicitly as $\pi_\theta(a | s)$ , and optimize the parameters $\theta$ either directly by gradient ascent on the performance objective $J(\pi_\theta)$ or indirectly by maximizing local approximations of $J(\pi_\theta)$. This optimization is almost always performed on-policy
<!--ID: 1686744089387-->


Q: What is does it mean if policy optimization is performed "on policy"
A: The gradient descent updates of $\pi_\theta(a | s)$ only use data collected while acting according to the most recent version of the policy. 
<!--ID: 1686744089391-->

Q: What is Q-learning?
A: Methods in this family learn an approximator $Q_\theta(s, a)$ for the optimal action-value function $Q^*(s, a)*$. Typically, they use an objective function based on the Bellman equation. This optimization is almost always performed off-policy.


Q: What does it mean if Q-learning is performed "off policy"?
A: Each update in learning the approximate $Q$ function can use data collected at any point during training, regardless of how the agent was choosing to explore the environment when the data was obtained.
<!--ID: 1686745562037-->


Q: How is the corresponding policy found in Q-learning?
A: $a(s) = \text{argmax}_a Q_\theta(s, a)$
<!--ID: 1686745562040-->



Q: What are the tradeoffs between policy optimization and Q-learning?
A: Policy optimization tends to be more stable because you are directly optimizing for the thing you want. Q-learning methods are less stable, but when they work they are more sample-efficient because they re-use data for effectively than policy optimization methods.
<!--ID: 1686745562042-->


Q: Explain pure-planning in the context of RL
A: Pure-planning never explicilty represents the policy, and instead uses something like MPC to select actions. Each time the agent observes the environment, it computes a plan which is optimal with respect to the model, where the plan describes all actions to take ove rsome fixed window of time after the present. The agent then executes the first action of the plan, and re-plans on the next iteration.
<!--ID: 1686745562045-->


Q: Describe expert iteration in RL
A: The agent uses a planning algorithm (like Monte-carlo tree search) in the model, generating candidate actions for the plan by sampling from its current policy. The planning algorithm produces an action which is better than what the policy alone would have produced (hence it is the expect relative to the policy). The policy is afterwards updated to produce an action like the planning algorithms output.
<!--ID: 1686745562048-->


Q: Describe "data augmentation for model-free methods" in RL
A: use a model-free RL algorithm to train a policy or Q-function, but either (1) augment real experiences with fictitious ones in updating the agent or (2) use only fictitious experiences for updating the agent
<!--ID: 1686745562050-->


Q: Describe "Embedding planning loops into policies" in RL
A: Embed a planning loop into the policy as a subroutine, so that complete plans become side information for the policy, while training the output of the policy with any standard model-free algorithm. They key concept is that in this frame work the policy can learn to choose how and when to use the plans. This makes model bias less of a problem, because if the model is bad for planning in some states, the policy can simply learn to ignore it.
<!--ID: 1686745562052-->

## Part 3: Intro to Policy Optimization

Q: Derive $\nabla_\theta J(\pi_\theta)$ (RL) in the case of un-discounted finite return.
A: Steps
1. $\nabla_\theta J(\pi_\theta) = \nabla_\theta E_{\tau \sim \pi_\theta}[R(\tau)]$, where $R$ is the return function and $\tau$ is a trajectory.
2. Expand expectation: $= \nabla_\theta \int_\tau P(\tau | \theta)R(\tau)$ 
3. Bring gradient under integral: $=\int_\tau \nabla_\theta P(\tau | \theta) R(\tau)$
4. Log derivative trick = $=\int_\tau P(\tau | \theta) \nabla_\theta \log P(\tau | \theta) R(\tau)$
5. $= E_{\tau \sim \pi_\theta}[\nabla_\theta \log P(\tau | \theta) R(\tau)]$
6. Note $\nabla_\theta \log P(\tau | \theta) = \nabla_\theta \log (\rho_0 (s_0) \prod_{t=0}^T P(s_{t+1} | s_t,a_t) \pi_\theta(a_t | s_t)) = \sum_{t=0}^T \nabla_\theta \log \pi_\theta(a_t | s_t)$
7. Therefore: $\nabla_\theta J(\pi_\theta) = E_{\tau \sim \pi_\theta} [\sum_{t=0}^T \nabla_\theta \log \pi_\theta (a_t | s_t) R(\tau)]$

Q: How does the loss function in RL policy optimization differ from loss functions in supervised learning?
A: Two differences:
1. A loss function is usually defined on a fixed data distribution which is independent of the model parameters we aim to optimize. Not so here, where the data must be sampled on the most recent policy
2. A loss function usually evaluates the performance metric that we care about. Here, we care about expected return, $J(\pi_\theta)$, but our "loss" function does not approximate this all, even in expectation. This "loss" function is only useful to use beacuse when evaluated at the current parameters, with data generated by teh current parameters, it has the negative gradient of the performance.


Q: What is the EGLP lemma?
A: The expected grad-log prob lemma:
Suppose $P_\theta$ is a parameterized probability distribution over a random variable $x$. Then:
$E_{x \sim P_\theta}[\nabla_\theta \log P_\theta (x)] = 0$


Q: Prove the ELGP lemma:  $E_{x \sim P_\theta}[\nabla_\theta \log P_\theta (x)] = 0$
A: Recall
$\int_x P_\theta(x) = 1$
Take the gradient of both sides
$\nabla_\theta \int_x P_\theta(x) = 0$
then use the log-derivative trick
$0 = \nabla_\theta \int_x P_\theta(x) = \int_x \nabla_\theta P_\theta(x) = \int_x P_\theta(x) \nabla_\theta \log P_\theta(x) = E_{x \sim P_theta} [\nabla_\theta \log P_\theta(x)]$


Q: Give the expression for the reward-to-go policy gradient, and explain how it is different from the regular policy gradient
A: Reward to go: $\nabla_\theta J(\pi_\theta) = E_{\tau \sim \pi_\theta}[\sum_{t=0}^T \nabla_\theta \log \pi_\theta(a_t | s_t) \sum_{t' = t}^T R(s_{t'}, a_{t'}, s_{t' + 1})]$, 
Regular: $\nabla_\theta J(\pi_\theta) = E_{\tau \sim \pi_\theta}[\sum_{t=0}^T \nabla_\theta \log \pi_\theta (a_t | s_t) R(\tau)]$
These two expressions are actually equivalent. The former makes it explicit that the gradient on the log-probs an action is only weighted by its reward-to-go, not the past return. In practive, the former is prefered, because it forces the terms reinforcing actions to be zero, instead of being zero in expectation.


Q: What is  a **baseline** in policy optimization
A: A consequence of the EGLP lemma is $E_{a_t \sim \pi_\theta}[\nabla_\theta \log \pi_\theta (a_t | s_t) b(s_t)] = 0$, which means we can add or subtract any number of terms from our expression for the policy gradient, without changing it in expectation:
$\nabla_\theta J(\pi_\theta) = E_{\tau \sim \pi_\theta} [\sum_{t=0}^T \nabla_\theta \log \pi_\theta (a_t | s_t) (\sum_{t' = t}^T R(s_{t'}, a_{t'}, s_{t' + 1}) - b(s_t))]$
The function $b$ is called a baseline

Q: What is the most common choice of baseline in policy optimization? Why?
A: The on-policy value function $V^\pi(s_t)$. This is the average return an agent gets if they start in state $s_t$ and acts according to policy $\pi$ for the rest of its life. 
Emperically, this reduces variance in the sample estimate for the policy gradient, resulting in faster and more stable learning. Conceptually, it should indicate that if an agent gets what it expected, it should feel "neutral" about it.

Q: Because the on-policy value function $V^\pi(s_t)$ cannot be computed exactly, how is it usually approximated in policy optimization?
A: Approximate it with a neural network $V_\phi(s_t)$, which is trained as such:
$\phi_k = \text{argmin}_{\phi} E_{s_t, \hat{R}_t \sim \pi_k} [(V_\phi(s_t) - \hat{R}_t)^2]$,
where $\pi_k$ is the policy at epoch $k$.

Q: The policy gradient takes the form $\nabla_\theta J(\pi_\theta) E_{\tau \sim \pi_\theta}[\sum_{t=0}^T \nabla_\theta \log \pi_\theta (a_t | s_t) \Phi_t]$. What are the five forms $\Phi_t$ could take?
A: Five forms
1. $\Phi_t = R(\tau)$
2. $\Phi_t = \sum_{t' = t}^T R(s_{t'}, a_{t'}, s_{t' + 1})$
3. $\Phi_t = \sum_{t' = t}^T R(s_{t'}, a_{t'}, s_{t'+1}) - b(s_t)$
4. $\Phi_t = Q^{\pi_\theta}(s_t, a_t)$: On-policy action-value function
5. $\Phi_t = A^{\pi_\theta}(s_t, a_t) = Q^\pi(s_t, a_t) - V^\pi(s_t)$: the advantage function of an action (how much better or worse that action it is than other actions on average). Here we are just using 4 with a baseline.



