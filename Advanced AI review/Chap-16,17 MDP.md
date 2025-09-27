> 在传统的搜索问题中，我们的后继函数直接确定了下一步的`state`。但是在真实世界中，绝大多数事情是不确定的，具有随机性。这意味着在一个状态下采取的操作可能导致多种可能的后续状态。这种不确定性搜索问题，可以用马尔可夫决策过程(`MDP`)模型来解决。

# Definition
* **A set of states $S$**. States in MDPs are represented in the same way as states in traditional search problems.
* **A set of actions $A$**. Actions in MDPs are also represented in the same way as in traditional search problems.
* A start state.
* Possibly one or more terminal states.
* Possibly a **discount factor** $\gamma$. 
* **A transition function $T(s,a,s')$**.It’s a probability function which
	represents the probability that an agent taking an action $a \in A$from a state $s \in S$ ends up in a state $s' \in S$.
* **A reward function** $R(s,a,s')$.Rewards may be **positive or negative** depending on whether or not they benefit the agent in question, and the agent’s objective is naturally to acquire **the maximum reward** possible before arriving at some terminal state.

`Markov`性体现在，对一个`state`$s_{k+1}$而言，其完全由上一个`state`$s_{k}$和`action`$a_{k}$决定。
![[Pasted image 20240108231824.png]]
所以，`transition function`$T(s,a,s')$表示的就是$P(s'|s,a)$.

# Search Tree

> 马尔可夫决策过程，就像状态空间图一样，可以形式化为搜索树。

通过引入`q-states`来对不确定性进行建模，本质上与`expectimax`中`chance node`相同。
![[Pasted image 20240108230954.png]]
其中，绿色节点表示`q-states`，表示已经在上一个`state`采取了`action`，但尚未解析为后继状态。
# Object Function
通过引入折扣因子，使得目标函数是一个有限实数。

![[Pasted image 20240108231504.png]]
![[Pasted image 20240108231731.png]]
# How to solve MDP？Bellman Equation

>在确定性、非对抗性搜索中，解决搜索问题意味着找到一个到达目标状态的最优**planning**。而解决`MDP`意味着找到一个最优策略$\pi^{*}$，也就是为每一个`state`找到一个合适的映射$s->a$，从而可以最大化`Utility`。

为了使用`Bellman Equation`，引入两个变量：
* The optimal value of a state $s$,$V^{*}(s)$.表示从$s$开始，之后每一步都按照最优策略进行行动，可以获得的`utility`.
* The optimal value of a `q-state`,$Q^{*}(s,a)$.表示从$s$开始执行动作$a$之后，后续每一步都按照最优策略进行行动，可以获得的`utility`.
## Definition of Bellman Equation
for `state`:(本质就是`maximize expected utility over all possible action from s`,equivalent to computing **the value of a maximizer node**)
$$V ^ { * } ( s ) = \max_{a} \sum _ { s' } T ( s , a , s ^ { \prime } ) \left[ R ( s , a , s ^ { \prime } ) + \gamma V ^ { * } ( s ^ { \prime } ) \right] $$
for `q state`:（本质就是`expected utility from q(s,a)`,equivalent to computing **the value of a chance node**))）
$$Q ^ { * } ( s , a ) = \sum _ { s ^ { \prime } } T ( s , a , s ^ { \prime } ) \left[ R ( s , a , s ^ { \prime } ) + \gamma V ^ { * } ( s ^ { \prime } ) \right] $$
可以发现：
$$V ^ { * } ( s ) = \max_{a \in A} Q ^ { * } ( s , a ) $$
## Value Iteration

![[Pasted image 20240109111808.png]]

### Problems
![[Pasted image 20240109113019.png]]

其中，第三点指的是在`value iteration`中，我们是通过判断`value`来确定是否收敛，但很多时候，在`value`还没有收敛时，`policy`就已经收敛了，此后的`value update`只不过是增加了可信度。
## Policy Iteration

> 在`Value Iteration`中，我们不断更新迭代`state`的`value`从而达到收敛的状态进行求解。但是，`MDP`的目标是获得最优的`policy`，为何不直接得到策略而是通过`value`来获得策略？

![[Pasted image 20240109114218.png]]
![[Pasted image 20240109114523.png]]
### policy evaluation
![[Pasted image 20240109113259.png]]
![[Pasted image 20240109112821.png]]

没有了`max`的要求，本质就是求解一个`linear equation`,可以采用迭代法或者直接求解解线性方程组。
![[Pasted image 20240109113349.png]]
### policy improvement/extraction

>Turn `Value` to `Policy`
![[Pasted image 20240109113636.png]]

## Summary
### Comparison
![[Pasted image 20240109113752.png]]

### Offline planning
![[Pasted image 20240109113954.png]]

`MDP`+`online trial`=`RL`
![[Pasted image 20240109114125.png]]