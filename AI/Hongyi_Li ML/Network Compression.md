# 概览
![[Pasted image 20250625154832.png]]

# Basic software techniques
## Network Pruning
#pruning
![[Pasted image 20250625155153.png]]

![[Pasted image 20250625155316.png]]

### weight pruning
![[Pasted image 20250625155757.png]]
### neuron pruning
![[Pasted image 20250625160253.png]]

#### Lottery Ticket Hypothesis（假说）
> 想象一个场景，训练一个work的模型《=》买彩票中奖1000元
> 买的越多，成功的可能越大
> 	* 积少成多（集成学习思想）
> 	* 容易出现单个彩票直接满足金额要求

![[Pasted image 20250625160537.png]]

![[Pasted image 20250625161340.png]]

![[Pasted image 20250625161702.png]]
## Knowledge Distillation
> student network将teacher network的输出作为groud truth

#knowledge_distillation
![[Pasted image 20250625161918.png]]

### Temperatured Softmax
> 更好的学习类别之间的关系（差异和联系）
> 在softmax函数中使用的温度参数。在机器学习尤其是深度学习里，softmax函数常用于将一个数值向量转换为表示各个类别概率的向量。而温度参数可以**调整softmax输出概率分布的“尖锐”程度。较低的温度值会使概率分布更集中，即模型的预测更加确定；较高的温度值会使概率分布更平滑，各个类别被选中的概率更接近**。例如在一些文本生成任务中，调整温度参数可以控制生成文本的多样性，较高温度可能生成更具创造性但准确性稍低的文本，较低温度则生成更保守、更符合常见模式的文本。 

![[Pasted image 20250625165206.png]]
## Parameter Quantization
![[Pasted image 20250625192554.png]]

![[Pasted image 20250625192715.png]]
## Architecture Design
### Depthwise Separable Convolution
> 深度可分离卷积。它是一种特殊的卷积操作，将常规卷积分解为深度卷积（Depthwise Convolution）和逐点卷积（Pointwise Convolution）两个步骤。深度卷积针对每个输入通道分别进行卷积，只在空间维度上进行操作，逐点卷积则通过1×1卷积核在深度方向上对深度卷积的输出进行融合，以生成最终的输出特征图。这种卷积方式能够**显著减少计算量和参数量，在保持模型精度的同时提高计算效率**，广泛应用于轻量级神经网络架构，例如在MobileNet中运用深度可分离卷积，在保证图像识别等任务精度的前提下，大幅降低了模型的运算量和存储需求，使模型能在移动设备等资源受限的环境中高效运行。 

#### Depthwise Convolution
> 卷积核没有厚度
> 考虑的是**同一个channel内部的关系**
> 和经典卷积核不同，filter的个数和input channels相同

![[Pasted image 20250625193943.png]]
#### Pointwise Convolution
> 考虑的是channels之间的联系
> filter的长宽为$1\times 1$

![[Pasted image 20250625194330.png]]

#### Comparison
![[Pasted image 20250625194825.png]]

#### why it works
![[Pasted image 20250625200712.png]]
![[Pasted image 20250625200724.png]]
## Dynamic Computation
![[Pasted image 20250625200944.png]]

### Dynamic Depth
![[Pasted image 20250625201131.png]]
### Dynamic Width
![[Pasted image 20250625201200.png]]
### Computation based on Sample Difficulty
![[Pasted image 20250625202017.png]]