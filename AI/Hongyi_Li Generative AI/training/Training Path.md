# Stage-1：Pretrain
#pretrain 
![[Pasted image 20250630111547.png]]
#self-supervised-learning 
## 数据清洗
![[Pasted image 20250630111755.png]]
## 问题
![[Pasted image 20250630112613.png]]

![[Pasted image 20250630112623.png]]
# Stage-2：Instruction Fine-tuning(SFT)
#SFT
## Instruction Fine-tuning
![[Pasted image 20250630112849.png]]

## Pretrain and fine-tune
![[Pasted image 20250630113243.png]]

### Adapter
> e.g. LoRA
> - 降低运算量
> - 保证参数不会和pretrain得到的参数偏差太多

#LoRA #adapter 
![[Pasted image 20250630113409.png]]

### Fine-tune
![[Pasted image 20250630113815.png]]
#### 垂直领域微调（专才）
![[Pasted image 20250630114027.png]]
#### 通用领域微调（通才）
![[Pasted image 20250630114217.png]]

![[Pasted image 20250630114311.png]]

##### Groud Truth data is important
![[Pasted image 20250630114729.png]]

![[Pasted image 20250630114906.png]]

### Open Source Model and Data
#### 逆向工程
#knowledge_distillation 
![[Pasted image 20250630122957.png]]

#### LLaMA
#LLaMA
![[Pasted image 20250630123011.png]]

![[Pasted image 20250630123132.png]]
# Stage-3：RLHF
![[Pasted image 20250630123646.png]]

## Instruction Fine-tune v.s. RLHF
### 数据产生过程中人类心智负担
> 生成->判别

![[Pasted image 20250630124256.png]]

### 模型学习过程中的贪心选择和全局度量
![[Pasted image 20250630124315.png]]

## 下围棋 v.s. 语言模型
==每一步本质上都是分类任务，整体看来是生成式任务（next token prediction）==
![[Pasted image 20250630124941.png]]

### stage-1 and stage-2
![[Pasted image 20250630125228.png]]
### stage-3
![[Pasted image 20250630125315.png]]

![[Pasted image 20250630125526.png]]

#### Reward Network
> 有效利用人类的回馈

##### Case-1
![[Pasted image 20250630125932.png]]
##### \*Case-2
![[Pasted image 20250630125946.png]]

![[Pasted image 20250630130555.png]]

## Other RLHF approaches
![[Pasted image 20250630130809.png]]
## RLHF->RLAIF
==LLM可能没有针对某个问题产生好的回答的能力，但是它可能具备判断回答是否“好”的能力==
![[Pasted image 20250630130921.png]]
## Problems
### multi-perspective
![[Pasted image 20250630131408.png]]
### human bias
![[Pasted image 20250630131428.png]]

# Summary
![[Pasted image 20250630131830.png]]
