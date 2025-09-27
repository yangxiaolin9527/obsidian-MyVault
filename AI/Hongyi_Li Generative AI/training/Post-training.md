> 狭义上：Alignment（SFT+RLHF/RLAIF）
> 广义上：在已有模型的基础上，进行进一步的训练，寻求（特定任务上）更好的表现

![[Pasted image 20250713215205.png]]

# Problem
> 在针对模型进行post-training后，模型会出现“灾难性遗忘”的问题（**对齐能力大幅下降**）
> 无论是pre-train style 或 SFT style

#adapter
![[Pasted image 20250713220510.png]]

#catastrophic_forgetting 
![[Pasted image 20250713220903.png]]

#LoRA 
![[Pasted image 20250713221128.png]]
![[Pasted image 20250713221502.png]]

# Solution
## Experience Replay

![[Pasted image 20250713222513.png]]

### If we are not access to the row data
> Get data from the trained model

![[Pasted image 20250713223055.png]]

![[Pasted image 20250713223413.png]]

### Paraphrase
> Post-training的时候，让label中的信息不偏离模型原始输出太多（正则化思想）

![[Pasted image 20250713225107.png]]
### Self-output
> 根据模型的回答，判断是否要进行保留并微调（RL思想）

![[Pasted image 20250713225119.png]]

![[Pasted image 20250713225553.png]]
# Summary
![[Pasted image 20250713230505.png]]