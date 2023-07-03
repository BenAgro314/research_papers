Title: Learning by Cheating
Authors: #Dian_Chen #Brady_Zhou #Vladlen_Koltun #Philipp_Krahenbuhl
Tags: #UT_Austin #Intel #CoRL
Year: 2019
Link: http://proceedings.mlr.press/v100/chen20a/chen20a.pdf

### Notes


### Anki

Q: What are the two stages of training in "Learning by Cheating"? Give the intuition as to why this works.
A: (1) Train an agent that has access to privileged information. (2) Train a purely sensory-motor agent based on the privileged agent.  Intuition: The (1) agent learns to act in a generalized way with privileged information, then (1) can provide richer supervision to (2) than just expert trajectories (DAgger-like training) where the agent can now learn to see, The agent in (1) can ask "what would you do if you had to do INSERT DRIVING COMMAND here?"
<!--ID: 1688348142183-->





