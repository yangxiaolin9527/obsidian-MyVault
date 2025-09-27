> 求解`MDP`是`offline`的一个例子，其中`agent`对$T(s,a,s')$和$R(s,a,s')$都有充分的了解，他们需要的所有信息都可以在`MDP`编码的世界中预先计算出最佳行动，而无需实际采取任何行动。
> In **online planning**, an agent must try **exploration**, during which it performs actions and receives **feedback** in the form of the successor states it arrives in and the corresponding rewards it reaps.
> The agent uses this feedback to estimate an optimal policy through a process known as **reinforcement learning** before using this estimated policy for **exploitation**, or reward maximization.

![[Pasted image 20240109132543.png]]

`Sample`:$(s,a,s',r)$
Often, an agent continues to take actions and **collect samples** in succession until arriving at a terminal state. Such a collection of samples is known as an **episode**. Agents typically **go through many**
**episodes during exploration** in order to collect sufficient data needed for learning.
# Model-Based Learning

> 既然$T$和$R$不知道，那就尝试使用在`exploration`过程中获得的样本来估计过渡和奖励函数，然后使用这些估计来解决`MDP`问题。

* **估计的本质**：大数定律，将多次试验的频率作为概率。
* **方法**：采样，统计，归一化，化为`MDP`。
* **思想**：使用`exploration`进行采样学习，估计转移和奖励函数；在学习一段时间后，使用`exploitation`最大化期望效益。
# Model-Free Learning

> 尝试直接估计`state`的$V(s)$或$Q(s,a)$，而不使用任何内存来构建`MDP`中的奖励和转换函数。

## Passive RL

> In passive reinforcement learning, an agent is given a policy to follow and learns the value of states under that policy as it experiences episodes, which is exactly what is done by policy evaluation for `MDPs` when $T$ and $R$ are known.
### Direct Evaluation

![[Pasted image 20240109163944.png]]
![[Pasted image 20240109163956.png]]
![[Pasted image 20240109164013.png]]

1. 同样是统计的方法，不过统计的是直接的`value`。在多次试验后，$V(s)$会不断逼近真实值。
2. **缺点**：
	* 每次试验没有考虑状态间的转换信息。在例子中，`B`和`E`都会经过`C`到达`D`从而获得最大的效用，但是只是因为一次偶然采样导致$V(E)$严重偏离真实值。当然，在足够多次数的试验后，$V(E)$终归会回归到真实值附近，但也浪费了许多时间。
	* 不知道$R$和$T$，无法进行`Policy Extraction`。

### Temporal-Difference Learning

> 怎样合理地使用`Bellman Equation`来更新$V(s)$？

一个想法：`Sample-Based`
![[Pasted image 20240109170037.png]]
**但是我们无法保证，一次采样后将`agent`重置至原位时，环境没有发生任何改变---忒修斯之船**
#### Exponential Moving Average
#EMA
![[Pasted image 20240109171012.png]]
![[Pasted image 20240109171037.png]]

1. **通过公式可以发现，越新的样本对结果的影响越大（可以理解为：随着样本的不断更新，计算出的值也越来越精确，理应发挥更大的作用）**
2. **缺点**：
	在使用移动平均获得$V(s)$后，我们想要据此更新`policy`,但是在`Bellman Equation`中，我们使用的是$\pi ^{*}(s)=\arg \max_{a} Q(s,a)$，想要知道$Q(s,a)$，我们仍然需要知道$R$和$T$，即使是估计值。所以只能结合一些`Model-Based`方法来使用。
## Active RL
### Q-Learning

> 直接一步到位，学习`Q-Value`。

![[Pasted image 20240109174511.png]]

1. **Off-policy** Learning:
	无论初始的`policy`是什么，在足够多次数的试验后，最终都会收敛到最优策略。因为在`Q`的`update`中，它是基于后续`Q`节点的最大`Q`值，而非`agent`真正采取的动作对应的`Q`。
2. 因为`max`的缘故，好的`value`会不断传播，因此`Q-Learning`甚至可以通过采取次优或随机行动直接学习最优策略。






