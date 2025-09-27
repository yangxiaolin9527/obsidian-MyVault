#model_editing

# Introduction
## 植入知识而非学习技能

![[Pasted image 20250715121520.png]]

## How to eval
#evaluation 
![[Pasted image 20250715121940.png]]

![[Pasted image 20250715122313.png]]

# Methods
![[Pasted image 20250715122735.png]]
## Without weight changing
### In-context Knowledge Editing（IKE）
> 在prompt中，告诉Model相关范例以及如何使用

![[Pasted image 20250715122540.png]]

![[Pasted image 20250715122644.png]]
## with weight changing
### Human-decision
#### ROME
> 寻找和输入最相关的部分，进行修改

[[Inside the LLM]]
![[Pasted image 20250715122956.png]]

![[Pasted image 20250715124458.png]]

#logit_lens #resdiual_stream 
![[Pasted image 20250715124804.png]]

#ROME
![[Pasted image 20250715131751.png]]
### AI-based editing
![[Pasted image 20250715131850.png]]
#### Idea
![[Pasted image 20250715132108.png]]
#### MEND
![[Pasted image 20250715133621.png]]