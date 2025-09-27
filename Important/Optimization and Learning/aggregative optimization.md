# Problem Formulation
$$
\min_{x \in \mathbb{R}^{m\times n}} F(x) = \sum_{i=1}^{m} f_i(x_i, g(x)),\:f_i(x_i, g(x)) = \mathbb{E}_{\varphi_i \sim P_{i,f}}[h(x_i, g(x); \varphi_i)]
$$
$$
g(x) = \frac{1}{m} \sum_{i=1}^{m} g_i(x_i),\:g_i(x_i) = \mathbb{E}_{\xi_i \sim P_{i,g}}[l(x_i; \xi_i)]
$$
## key
总的目标是最小化所有个体的目标$f_{i}$的总和，每个$f_i$取决于个体变量$x_i$和共享变量$g(x)$，后者是所有个体另一个目标$g_i$的平均值（共识变量，从所有个体中聚合而来的信息）。
1. 如何定义$g(x)$的意义
2. 如何展开$h(x_i, g(x);\varphi_i)$
# PFL with consensus regularization
>Li T, Hu S, Beirami A, et al. [Ditto](https://proceedings.mlr.press/v139/li21h/li21h.pdf): Fair and robust federated learning through personalization[C]//International conference on machine learning. PMLR, 2021: 6357-6368.
## problem setup
FL中，m个客户端在保持数据去中心化的同时协同训练一个模型，PFL旨在解决全局模型的泛化性问题，每个客户端结合全局信息和本地信息训练一个个性化模型。==这样鼓励客户端将本地模型与全局信息对齐，减少方差（公平性），提高泛化能力==
1. 当共识信息为模型均值时，即$g(x)=\frac{1}{m}\sum_{i=1}^{m}x_i$, 客户端目标$$f_i\left( x_i, g\left( x \right) \right) =\mathbb{E} _{\varphi _i\sim \mathcal{P} _{i,f}}\left[ h\left( x_i, g\left( x \right) ; \varphi _i \right) \right] =\mathbb{E} _{\varphi _i\sim \mathcal{P} _{i,f}}\left[ l\left( x_i;\varphi _i \right) +\lambda \left( x_i-g\left( x \right) \right)^2 \right] $$
2. 当共识信息为全局平均损失时，即$g(x)=\frac{1}{m}\sum_{i=1}^m{g_i(x_i)}=\frac{1}{m}\sum_{i=1}^m{\mathbb{E} _{\xi _i\sim \mathcal{P} _{i,g}}\left[ l\left( x_i; \xi _i \right) \right]}$，客户端目标为
	$$f_i\left( x_i, g\left( x \right) \right) =\mathbb{E} _{\varphi _i\sim \mathcal{P} _{i,f}}\left[ h\left( x_i, g\left( x \right) ; \varphi _i \right) \right] =\mathbb{E} _{\varphi _i\sim \mathcal{P} _{i,f}}\left[ l\left( x_i;\varphi _i \right) +\lambda \left( g_i(x_i)-g\left( x \right) \right)^2 \right]
	$$
	问题在于，这里的$\varphi_i$和$\xi_i$分别代表什么

## 0424 TODO
* [x] 数据集异构处理
* [x] 每个agent需要有自己本地的测试集
* [x] 计算测试准确率使用各agent在本地测试集上的表现的平均（即acc平均）
* [x] 增加DSGD(Consensus,各agent模型最终会趋同?)的对比算法（采用模型平均）

## 0504
* [x] 流数据形式处理
    直接设置`batch_size=1`
* [x] 实现算法Algorithm1
* [x] cli组处理
* [x] 参数和优化器初始化逻辑

## 0506
* [ ] 定位显存占用大原因：**self.historical_data**
* [ ] 进入新的epoch时，是否需要重置历史数据（重复、内存问题）
* [ ] 流数据形式显存占用大，MNIST上效果和`mini-batch`形式的几乎无差别

## 0508
* [x] 将DAGT算法修改为AODGT
* [ ] 实现算法APDGT
    $\lambda_x=1$过大，loss震荡，调小至0.1
* [x] 实现算法1
* [x] 实现算法jfz_alg
* [ ] 实现算法huang_alg
    $\sigma_0=1$过大，loss极大，调小至0.001

## 0509
* [ ] ==流数据形式下，单个样本下立即过拟合==
* [x] ==`mini-batch`设定下==：group1中：ring图设定下，只有alg1可以跑起来（但是cifar增长缓慢）；全连接图设定下，都可以跑起来
# Fair Resource Allocation in Distributed Systems
## problem setup
类似于之前的EV charge问题，m个agent分配资源，$g(x)$代表平均的资源使用量。每个agent的目标为最小化$\mathbb{E} _{\varphi _i\sim \mathcal{P} _{i,f}}\left[ h\left( x_i, g\left( x \right) ; \varphi _i \right) \right]$，其中的$h(\cdot)$包括本地的使用量和正则项，防止过度利用/公平性。  
