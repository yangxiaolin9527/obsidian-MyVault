# 概要
#GAN
## Generator
![[Pasted image 20250620160021.png]]

**在某些任务中，同样的输入：输出存在不同的可能**
![[Pasted image 20250620160450.png]]

**让输出服从一个分布---->1对多，例如在处理具有“创造性”任务（Drawing、ChatBot）时**
![[Pasted image 20250620160535.png]]

# Unconditional GAN
## generation
> 只有simple distribution的输入

![[Pasted image 20250620161423.png]]

## Discriminator
![[Pasted image 20250620161540.png]]

## Adversarial idea
![[Pasted image 20250620161810.png]]
## Algorithm
### train discriminator
![[Pasted image 20250620162140.png]]
### train generator
![[Pasted image 20250620162350.png]]
### 流程框架
![[Pasted image 20250620162556.png]]
## Theory
### Object
> 学习联合概率分布--->最小化分布间的差异

![[Pasted image 20250620163411.png]]
### Divergence between two distributions
> 不知道分布的表达式，但是可以采样。再通过Discriminator进行计算

![[Pasted image 20250620163723.png]]

![[Pasted image 20250620164109.png]]
![[Pasted image 20250620165008.png]]

#### （bilevel）Optimization
![[Pasted image 20250620165209.png]]

## Train tips
> GAN is difficult to train

![[Pasted image 20250620165632.png]]

### WGAN
#### Problem of JS divergence
![[Pasted image 20250620165802.png]]

#### Wasserstein divergence
> 对称、平滑

![[Pasted image 20250620170219.png]]
**问题：需要穷尽所有的策略，计算复杂度高**

#### 1-Lipschitz约束
![[Pasted image 20250621185637.png]]

![[Pasted image 20250621185919.png]]

### GAN for Sequence Generation

![[Pasted image 20250621191620.png]]
### More Generative Models
![[Pasted image 20250621192013.png]]

### Evaluation of Generator
![[Pasted image 20250621192544.png]]
#### Mode Collapse
> Diversity Problem

![[Pasted image 20250621192750.png]]

#### Mode Dropping
![[Pasted image 20250621192959.png]]

#### Quality and Diversity
> 目标：高内聚（一张图片的分布集中），低耦合（多张图片的标记均匀、分散）

![[Pasted image 20250621193411.png]]

![[Pasted image 20250621193637.png]]

#### Memory Problem
![[Pasted image 20250621194045.png]]

# Conditional GAN
#conditional_GAN

## 任务类型
### Text-to-image
![[Pasted image 20250621194323.png]]

![[Pasted image 20250621194459.png]]

![[Pasted image 20250621194644.png]]
### pix2pix
![[Pasted image 20250621194954.png]]

![[Pasted image 20250621195150.png]]

### sound-to-image
![[Pasted image 20250621195311.png]]

### Talking Head Generation
![[Pasted image 20250621195521.png]]

### Multi-label Classifier
> 在某些情况下，多标签图像分类器可以被视为条件生成器的一种特例。具体来说，输入图像 $x$ 可以被视为条件 $c$，而多标签分类器的输出 $\hat{y}$ 则是“生成”的结果。因此，多标签分类器实际上是在“生成”一个标签分布，条件是输入图像。这种视角的一个好处是，它允许我们使用生成模型中的技术来改进多标签分类器。例如，可以使用生成对抗网络的思想来增强分类器的鲁棒性，或者通过变分推断来提高模型的泛化能力。

![[Pasted image 20250621195650.png]]

# Unsupervised Conditional Generation
> If we don't have **any** labeld data

## Example
![[Pasted image 20250621195914.png]]

![[Pasted image 20250621201735.png]]

## Cycle GAN
#cycle-gan
> 因为没有labeled data，无法使用conditional GAN中的训练技巧（针对输出和condition不匹配的情况,给予低分）

![[Pasted image 20250621200429.png]]

理论上存在问题：如何保证输入和输出的相似度（极端情况，两个Generator都只进行左右翻转）
工程上：这样的问题较少出现
![[Pasted image 20250621201015.png]]
![[Pasted image 20250621201939.png]]
### 模型框架
![[Pasted image 20250621201419.png]]