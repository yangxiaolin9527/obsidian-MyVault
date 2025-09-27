# Introduction

![[Pasted image 20250714152419.png]]
## Test-Time Compute
> Inference 时计算

#reasoning
![[Pasted image 20250708102221.png]]
### Test-Time Scaling
> 深度不够、长度来凑
> A brute but useful way: 将[END]替换为[wait]

![[Pasted image 20250708102249.png]]

![[Pasted image 20250714152938.png]]

# Better Chain-of-Thought(COT)
#COT 
典型：`Let's think step by step.`
![[Pasted image 20250714153120.png]]

## Supervised COT
> 手动将足够细致的推理过程描述写在prompt中
> ---不是所有的模型都可以通过这种方法实现近似的reasoning效果

![[Pasted image 20250714153710.png]]

# Explore

![[Pasted image 20250714154243.png]]

## Majority Vote
> 集成学习的思想?

![[Pasted image 20250714154444.png]]
## Verification
![[Pasted image 20250714154626.png]]

### Verfication step by step
> 对回答进行剪枝，不需要针对每一个output都直接到“叶子节点”才进行verification

![[Pasted image 20250714155130.png]]
#### How to train Process Verifier
![[Pasted image 20250714155238.png]]
##### Search based on the score
#beam_search 
![[Pasted image 20250714155350.png]]

![[Pasted image 20250714155729.png]]

# Post-training-based Methods
![[Pasted image 20250714155901.png]]

## Imitation Learning
![[Pasted image 20250714155952.png]]

### Generate reasoning process data
> 存在问题：可能会出现结果正确但是推理过程错误的现象。

![[Pasted image 20250714160202.png]]
#### 但是是否真的需要推理过程的每一步都正确?
![[Pasted image 20250714160445.png]]
#### Look back and rechoose
> 回溯、剪枝思想

![[Pasted image 20250714160804.png]]
#### Journey learning
![[Pasted image 20250714161103.png]]

### Knowledge-Distillation
#knowledge_distillation 
![[Pasted image 20250714161304.png]]

![[Pasted image 20250714161412.png]]
## Result-oriented reasoning based on RL

![[Pasted image 20250714161530.png]]

### Train with Majority Vote
> 多种Reasoning技巧，可以混合使用

![[Pasted image 20250714161920.png]]

### Usable DeepSeek R1
> R1-zero 在训练过程中，只注重结果，导致其推理过程是人类难以读懂的

![[Pasted image 20250714163000.png]]

### Foundation model is important
> RL是**强化模型原有的能力**

![[Pasted image 20250714163229.png]]