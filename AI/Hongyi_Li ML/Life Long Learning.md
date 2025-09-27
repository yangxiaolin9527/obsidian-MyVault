# 概论

![[Pasted image 20250625103727.png]]

## 灾难性遗忘（Catastrophic forgetting）
#catastrophic_forgetting 
![[Pasted image 20250625104131.png]]

![[Pasted image 20250625104514.png]]

## What about multi-task learning
> 将所有的数据放在一起训练（每次面对新任务，都有复习前面所有的任务）
> 1. 存储问题
> 2. 计算问题

![[Pasted image 20250625142641.png]]

## What about learning more models
![[Pasted image 20250625142859.png]]

## Life-Long v.s. Transfer
![[Pasted image 20250625143234.png]]

## How to evaluation

![[Pasted image 20250625143800.png]]

![[Pasted image 20250625144009.png]]

# Research directions
## *Selective Synaptic Plasticity
> 选择性突触可塑性
> **Regularization-based** Approach

![[Pasted image 20250625144429.png]]

$b_i$作为**超参数**，不是learn出来的（机器会将它直接学成0）
![[Pasted image 20250625145254.png]]

### Fine-tune $\theta_i$ to adjust $b_i$
![[Pasted image 20250625151832.png]]

![[Pasted image 20250625153218.png]]

![[Pasted image 20250625153641.png]]
## Additional Neural Resource Allocation
> 从小开始逐步增加网络/从大开始选择小网络

![[Pasted image 20250625154024.png]]
## Memory Reply

![[Pasted image 20250625154252.png]]

![[Pasted image 20250625154430.png]]

## Other scenarios
![[Pasted image 20250625154541.png]]

## Curriculum Learning（what is the proper learning order?）
![[Pasted image 20250625154646.png]]
