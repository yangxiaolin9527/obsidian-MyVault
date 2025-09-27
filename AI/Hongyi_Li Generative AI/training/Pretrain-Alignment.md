![[Pasted image 20250712135150.png]]

# Introduction
## Pretrain is important but not enough
#pretrain
> Model-**base**：pretrain only
> Model-chat/instruction：after alignment

![[Pasted image 20250712135639.png]]

## Alignment:Quality is all you need
![[Pasted image 20250712140110.png]]

# Alignment

## How to get HQ data for alignment?
#knowledge_distillation 

![[Pasted image 20250712141127.png]]
### Where are the Questions from?
> Maybe they are not important

==学Groud Truth 不如直接学“老师”的答案？==
![[Pasted image 20250712141408.png]]

![[Pasted image 20250712141540.png]]

## What does SFT do for the model
#alignment
![[Pasted image 20250712141914.png]]

### So, is it easy to align?
![[Pasted image 20250712142927.png]]
#### self-alignment
#self-alignment
![[Pasted image 20250712143118.png]]

## The limitations of alignment
> 出现了回答格式正确，内容错误的现象

![[Pasted image 20250712150226.png]]
### Learn unknown things may be bad
> pretrain得到的知识，限制了model的上限；Alignment强化已经知道的知识，练习可能知道的知识。
> Alignment对**model有能力，但是不知道如何正确输出的问题**，最有帮助：==RL作为Alignment有效方式的原因==

#alignment 
![[Pasted image 20250712150407.png]]

### Aligtment dose not change the model thoroughly
> 从residual stream理论中的logit lens技术解释实验结果：Alignment主要改变的是权重$k$,而不是模型可以表示的语义$v$。--》如果我们可以利用模型编辑技术/representation engineering...

#logit_lens 
![[Pasted image 20250712151903.png]]
# Pretrain
## Data
### Quality
#### polyform data
![[Pasted image 20250712143640.png]]
#### clean data
![[Pasted image 20250712145354.png]]

#### various data
![[Pasted image 20250712145532.png]]
![[Pasted image 20250712145717.png]]
### quantity
![[Pasted image 20250712144014.png]]

#FineWeb
![[Pasted image 20250712145017.png]]
![[Pasted image 20250712145826.png]]
# Summary
![[Pasted image 20250712152437.png]]
