> 解决决策问题
> 特别是人类不知道最佳label的情况（类比于supervised learning）

# What is RL
> Actor、Environment、Reward、Policy

![[Pasted image 20250627103928.png]]

## ML perspective

### Model
![[Pasted image 20250627104941.png]]
### Loss
![[Pasted image 20250627105143.png]]
### *Optimization

1. Reward和Env不是Network
2. actor的action具有随机性
![[Pasted image 20250627105721.png]]

# Policy Gradient
## How to control your actor
![[Pasted image 20250627110545.png]]

### Two problems
![[Pasted image 20250627110552.png]]

### Version-0
![[Pasted image 20250627113008.png]]

### Version-1
![[Pasted image 20250627113255.png]]
### Version-2
![[Pasted image 20250627113441.png]]
### Version-3
![[Pasted image 20250627113939.png]]
**Q: How to design `b`?**

## Algorithm
![[Pasted image 20250627114257.png]]

![[Pasted image 20250627114351.png]]

==不同于supervised learning，RL中数据和Network是耦合的（network的输出action会影响从environment中获得的observation）==
![[Pasted image 20250627115023.png]]

#### On-policy RL v.s. Off policy-RL
#ppo
> 近端策略优化算法（Proximal Policy Optimization）简称 PPO ，是 OpenAI 开发的一系列无模型强化学习算法，旨在优化策略网络以最大化累计奖励。例如在机器人控制、自动驾驶模拟等场景中训练智能体。

![[Pasted image 20250627115255.png]]

### Exploration v.s. Exploitation
#### Exploration
**No try no more gain**
![[Pasted image 20250628154441.png]]
# Actor-Critic
## Critic
![[Pasted image 20250628155100.png]]

## Monte-Carlo-based approach
![[Pasted image 20250628155300.png]]
## Time-difference（TD）approach
![[Pasted image 20250628155527.png]]

## Version-3.5
![[Pasted image 20250628160301.png]]

![[Pasted image 20250628160612.png]]

## Version-4 (Advantage Actor-Critic)
![[Pasted image 20250628160707.png]]

![[Pasted image 20250628161029.png]]

## Critic-only (DQN)
![[Pasted image 20250628161125.png]]
# Reward Shaping
## Sparse Reward
![[Pasted image 20250628161712.png]]

## reward shaping
![[Pasted image 20250628162305.png]]

### curiosity
![[Pasted image 20250628162414.png]]
# No Reward: Learning from Demonstration
![[Pasted image 20250628163029.png]]

## Imitation Learning
![[Pasted image 20250628163607.png]]
![[Pasted image 20250628163626.png]]

## Inverse Reinforcement Learning
### Basic Idea
**Learning Reward function**
![[Pasted image 20250628163758.png]]

![[Pasted image 20250628164233.png]]

### IRL and GAN
![[Pasted image 20250628164346.png]]


