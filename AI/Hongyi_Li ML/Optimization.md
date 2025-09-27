# loss和local minimal
#local_minimal
loss不下降 critical_point--->**local_minima** or **saddle_point**
![[Pasted image 20250612140303.png]]

> 如何判断error surface的形状？
![[Pasted image 20250612160248.png]]

**判断$v^THv$->$H$的正定型**
理论上可行，工程上难以操作：
![[Pasted image 20250612161905.png]]

==DL中，local minimal在高维空间实际上很少见，很多是时候都是saddle point==
![[Pasted image 20250612162806.png]]

# batch和momentum
> 解决local minimal和saddle point的问题
## batch
### 相关概念
epoch
batch
batch size
iteration
mini-batch,full-batch
### small batchsize v.s. large batchsize
**GPU并行计算**
![[Pasted image 20250612164519.png]]
![[Pasted image 20250612164701.png]]
**noisy but useful(robust)**
![[Pasted image 20250612164819.png]]
==not model bias, optimization fails instead==
![[Pasted image 20250612165359.png]]
## momentum
#momentum
* 视角1：当前梯度+前一步方向
* 视角2：考虑历史梯度信息
![[Pasted image 20250612170058.png]]

# learning rate
> 影响参数更新的两个因子：gradient and lr

Loss不下降，gradient_norm还在变化----》**既不在local_minimal，又不在saddle_point**
![[Pasted image 20250613110404.png]]
## 问题
==如果接近critical point?==
固定的lr很难训练
![[Pasted image 20250613110931.png]]
## Adagrad:Root Mean Square
![[Pasted image 20250613111506.png]]
## RMSProp: EMA
![[Pasted image 20250613112100.png]]
## ADAM: RMSProp+Momentum
![[Pasted image 20250613112147.png]]
## Learning rate scheduling
![[Pasted image 20250613112846.png]]
Warm Up: ResNet, Transformer

## Summary and Guidance
![[Pasted image 20250613113149.png]]
# loss function
见概率机器学习中，分类与回归问题、OLS、MSE和CE、Sigmoid和Softmax
# batch normalization
**输入的维度对optimization的影响**（畸形的error surface）--->**需要Normalization**
![[Pasted image 20250615120230.png]]
==可以在激活函数前/后进行==
#batchnorm
![[Pasted image 20250615121807.png]]

![[Pasted image 20250615122037.png]]
Note:
1. $\mu$和$\sigma$是模型中不可学习的参数(与输入有关)
2. **通常需要在Normalization后进行一个线性化**，$\tilde{z}^i \to \hat{z}^i$ ,参数$\beta$和$\gamma$的初始值通常为$\boldsymbol{1}^n$和$\boldsymbol{0}^n$