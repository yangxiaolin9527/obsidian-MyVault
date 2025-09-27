# key points
Based on FedAvg, 结合MAML方法，实现一个个性化联邦元学习的框架，重点用于解决数据(non-iid)，系统异质性的问题

**理论**部分，建立了该框架下的对于**非凸损失函数**的收敛性性分析，特别地，使用两种不同的分布间距离度量从而量化client之间的数据异质性程度。

相较于之前的FL+MAML方法（最早的? 2018 `Federated Meta-Learning with Fast Convergence and Efficient Communication`, 只有数值实验结果），per-FedAvg中，本地节点执行了多次更新后才将梯度发送到中央聚合点。

没有考虑到框架的鲁棒性和隐私性

