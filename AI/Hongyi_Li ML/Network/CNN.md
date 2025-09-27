# Neuron Version Story
## 感受野（receptive field)与卷积层
![[Pasted image 20250616212041.png]]
### 经典设置
#### 分组、局部感受野
![[Pasted image 20250616212312.png]]

#### 不同感受野可以共享参数（filter）
![[Pasted image 20250616212704.png]]
![[Pasted image 20250616212803.png]]

### 卷积层
![[Pasted image 20250616213151.png]]

# Filter Version Story
## Filter和Feauture Map
![[Pasted image 20250616213511.png]] 
## 卷积层的叠加
通道为c的$n \times n$的输入经过一层具有$m$个filter的卷积层后，得到的是$p\times p$的$m$通道的tensor，
其中$p$的大小与stride、padding和该层输入的长和宽有关。
![[Pasted image 20250616213946.png]]
# 下采样与Pooling
> Pooling 是一个操作，而不是可学习的参数

--->**减小计算的同时，也丢失了信息**（但计算资源足够时，有时也可以选择不使用）
![[Pasted image 20250616214747.png]]

# Whole CNN
![[Pasted image 20250616214951.png]]

## 适用任务
**网络设施需要为具有图像特点的任务而服务，并且根据任务设定网络和参数**
![[Pasted image 20250616215604.png]]

## 缺点
CNN具备**平移不变性**，但是不具有**旋转不变性**和**尺度不变性**---->==Data Augment==，网络优化
![[Pasted image 20250616215704.png]]
