#model_merging
# Introduction
## 让模型学习一个技能
### Common Case
> 获取原始数据+大量资源进行微调

![[Pasted image 20250715134630.png]]
### Model Merging
![[Pasted image 20250715134853.png]]
### Task Vector
#task_vector

![[Pasted image 20250715135143.png]]
# Application of Task Vector
## Add
![[Pasted image 20250715135342.png]]

![[Pasted image 20250715140045.png]]
## Minus
#machine_unlearning
![[Pasted image 20250715140347.png]]

![[Pasted image 20250715140503.png]]
## Analogy
![[Pasted image 20250715140649.png]]

![[Pasted image 20250715141140.png]]
# Lesson
> 不同任务model从base-model上SFT得来，如果SFT所修改的参数相关性越低，Model Merging成功的概率就会越高

## Task-models' weight correlation
![[Pasted image 20250715141547.png]]

![[Pasted image 20250715141726.png]]
## Model's scale
![[Pasted image 20250715142052.png]]
