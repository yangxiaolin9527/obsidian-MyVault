>As opposed to normal search, which returned a comprehensive **plan**, adversarial search returns a **strategy**, or **policy**, which simply recommends the best possible move given some configuration of
our agent(s) and their adversaries
# MiniMax

 > 悲观思维，以对手不会犯错，总是做出最优选择为前提进行决策。
 
 ## Alpha-Beta Pruning
 $\alpha-\beta$剪枝只是优化搜索树，对结果没有影响。
 ## Example
 ![[Pasted image 20240108141937.png]]
 ![[Pasted image 20240108141952.png]]
 **Solved**
 ![[Pasted image 20240108143127.png]]
 ![[Pasted image 20240108142519.png]]
# Expectimax
类似于软分类和硬分类的区别，使用`minimax`在以下这种情况中，过分谨慎会失去一些好的机会：
![[Pasted image 20240108141648.png]]
引入`chance`节点，它不是像最小化节点那样考虑最坏情况，而是考虑期望情况。更具体地说，最小化者只是计算其子节点的最小效用，而机会节点计算期望效用或期望值（引入概率）。
## Example
![[Pasted image 20240108143721.png]]
**Solved**
![[Pasted image 20240108143739.png]]
# AlphaGo

> AlphaGo的做法是使用了`MCTS`与两个深度神经网络相结合的方法，一个是以借助估值网络（`value network`）来评估大量的选点，一个是借助走棋网络（`policy network`）来选择落子，并使用强化学习进一步改善它。
![[Pasted image 20240108144053.png]]