# Decentralized (Federated)Learning
## DFL and CFL
Differences：
1. key: 有无用于聚合clients 传来的update 的中央服务器
2. how to update model: （CFL）中央服务器聚合客户端更新（各种算法解决如何更新）并产生更新后模型，再将更新后**全局模型**同步到所有的客户端。
   （DFL）因为不存在中央服务器，各clients的模型通过不断迭代后达到一个**consensus**的状态（各种算法解决客户端如何communicate，和谁交互信息，交换后如何更新/聚合）
3. 拓扑图不同: CFL是单server的C/S架构拓扑，DFL是P2P架构。
4. 通信协议不同: CFL中经典的如FedAvg算法，DFL中有广泛用于Decentralized Learning的算法如Push-sum，Metropolis rule/hastings（都属于特定的**Gossip Algorithm**）

>In contrast, we consider a decentralized scheme where there is no central node and only localized communications with neighbors occur. This leads to a more scalable and flexible system and avoids communication bottleneck at the central processor.
>
>the central server in an FL setup is a potential point of weakness: it could fail or be maliciously attacked, which would make the distributed
 learning fail.

## Fundamentals of DFL
### Architecture
1. Cross-silo: 在跨孤岛分布式联邦学习（DFL）中，节点是组织或数据中心。通常节点的数量相对较少（少于100个），每个节点拥有大量数据（大约数百万样本），这些数据可能是从不同业务中的消费者那里分布和汇总而来的。同时，节点在一段时间内使用一致、稳健且可扩展的计算能力。同样，这些节点在网络中具有高性能，避免了节点之间通信时的故障点。例如，在不同的医院中应用，通过训练一个联邦模型来进行肿瘤分类，同时保持他们的正电子发射断层扫描（PET）图像本地存储。

2. Cross-device: 在跨设备分布式联邦学习（DFL）中，节点的数量相对较多（超过100个），每个节点的数据量相对较小（大约数千样本），且计算能力有限。由于对功耗的担忧，不能要求单个设备执行复杂的训练任务。此外，节点可能会周期性地断开网络连接，从而显著增加网络掉线率，负面影响DFL的性能。节点通常是边缘设备或机器人，如无人机（UAV），这些设备之间的通信通常较弱，除非保持在一个较近的覆盖半径内。

## Reference
1. [Decentralized federated learning_ of deep neural networks on non-iid data](https://cz5waila03cyo0tux1owpyofgoryroob.aminer.cn/59/53/A5/5953A5DF43CDA2DAEF1BCF76D03562B8.pdf#page=4.08)
2. [Dif-MAML: Decentralized Multi-Agent Meta-Learning](file:///C:/Users/YangHL/Desktop/Dif-MAML_Decentralized_Multi-Agent_Meta-Learning.pdf#page=3.75)
# DP for Privacy-preserving
> 隐私保护性作为(D)FL中的一个安全属性，DP是一个有效的隐私保护手段，解决(D)FL中的Privacy security问题。


# Meta-learning and PFL
> Meta-Learning思想与 PFL的设置相似，用于解决(D)FL中的 non-iid 问题。

