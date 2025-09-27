# 5个基本概念
* *state space*:事物所有可能状态的抽象表示
* *successor function*:`state`作为输入，计算该状态下执行动作的成本并返回后继状态
* *start state*:`agents`在状态空间中的初始状态
* *goal test*:`state`作为输入，判断该状态是否为目标状态
* *cost function*:标记在状态图的节点之间的边上，表示执行该边对应后继函数的代价
# 搜索方法
## 性能度量
* **完备性** - if there exists a solution to the search problem, is the strategy guaranteed to find it given infinite computational resources?
* **最优性**- is the strategy guaranteed to find the lowest cost path to a goal state?
* **复杂性**
	* ==分支因子==$b$
	* ==搜索树最大深度== $d$
	* ==The depth of the shallowest solution== $m$
## Uninformed Search
### DFS
### BFS
### UCS
### DLS

限定深度的深度优先搜索。
### IDS
迭代加深（Iterative deepening）搜索是深度限制搜索的升级版本，即首先允许深度优先搜索K层搜索树，若没有发现可行解，再将K+1后重复以上步骤搜索，直到搜索到可行解。
在迭代加深搜索的算法中，连续的深度优先搜索被引入，每一个深度约束逐次加1，直到搜索到目标为止。
### Bi-direction Search
分别从初态和终态启动两个`BFS`，分别维护两个未拓展节点集合,当两个集合有交集时算法结束。
这种算法没有带来根本性的变化，且在某些状态些不方便定义终态的“前驱函数”（例如象棋的目标状态）。但是提供了一种搜索算法改进的框架，并能获得极大的优化，同时完备性和最优性和基准算法一致。
### Summary
![[Pasted image 20240108103800.png]]
## Informed Search

这里需要设计一个评估函数$f$,将搜索树的节点$N$映射到一个非负实数$F(n)$，从表示从初始状态到达某个节点的路径耗散（越小越好）。
根据设计的$f$，可以分为两种算法：
* $f(N)=h(N)$:从当前节点到目标节点的路径耗散，对应**Greedy Algorithm**
* $f(N)=g(N)+h(N)$:$g(n)$表示从初始节点到当前节点的路径耗散，$h(n)$表示从当前节点到目标节点的路径耗散，对应$A^{*}$**算法**。
事实上，我们并不知道真正的$h^{*}(N)$，使用启发式函数进行逼近。
### heuristic function
#### Admissible

> A heuristic $h(N)$ is admissible if for every node $N$,  $0 ≤ h(N) ≤ h^{*}(N)$, where $h^{*}(n)$ is the true cost to reach the goal state from n.

**其核心观点是“乐观”地估计路径耗散。**
#### Consistent

> 启发式函数需要满足三角不等式

![[Pasted image 20240108110228.png]]

#### Domination
![[domination in heuristic.png]]

# 例题
## example-1
![[Pasted image 20240108110954.png]]

[Solved](C:/Users/YangHL/Desktop/Advanced%20AI/homework/hw3/23220230156597_杨宏林_作业03.pdf)
## example-2 传教士野人问题
![[Pasted image 20240108111357.png]]

[Solved](C:/Users/YangHL/Desktop/Advanced%20AI/homework/hw3/23220230156597_杨宏林_作业03.pdf)
