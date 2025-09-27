# I. Introduction
FL->PFL (non-iid)
DP+PFL 损失函数的凸性？

Contribution：
1. `RDP-PFL, the first work of its kind that provides a theoretical analysis of the convergence properties of PFL combined with RDP and meta-training.
2. meta-learning-based PFL+RDP, RDP composition theory for Privacy preserving in the two-stage procedure of meta-learning.

# II. Preliminaries
## A. Framework of PFL
将meta-Learning（MAML）的思想运用到联邦学习中，每一个agent具有用于快速适应的training set和用于更新global model的validation set.
在训练阶段，每一个agent在接受到global model后，自己进行若干epoch的元训练（每一个epoch中包含adaptation和validation），之后再将更新后的参数/(累积梯度是否可行？)发送到中央服务器进行集中更新。

**Note**:
1. paper中，似乎将support set和query set的使用混淆了？

## B. Renyi Differential Privacy
使用Renyi divergence可以获得比$\epsilon-DP$更加宽松的DP定义，同时避免像$(\epsilon, \delta)-DP$定义那样出现灾难性失败的可能性。

# III. Differentially Private Personalized Federated Learning

# IV. Convergence Analysis

# V. Experiments

## A. Experimental Setup

## B. Evaluation of Privacy Levels

# VI. Related Works

## A. Federated Meta-Learning

## B. Machine Learning With Differential Privacy

# VII. Conclusion
`