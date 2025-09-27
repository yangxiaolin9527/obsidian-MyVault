> 结构化数据生成

![[Pasted image 20250706144115.png]]

# 生成式AI的本质

![[Pasted image 20250706144426.png]]

# 生成策略
## autoregressive
#autoregressive 
### For NLP
![[Pasted image 20250706144801.png]]
### For image and voice
![[Pasted image 20250706145329.png]]
## non-autoregressive (NAG)
> NAG可以利用并行计算的优势

![[Pasted image 20250706145459.png]]
### for NLP
> Two basic ways

![[Pasted image 20250706145627.png]]
#### Multi-modality problem
![[Pasted image 20250706145852.png]]

### for image and voice
#### same random vector
#GAN 
![[Pasted image 20250706151343.png]]
#### AR + NAR
![[Pasted image 20250706151631.png]]

##### AR and auto-encoder
#auto-encoder 
![[Pasted image 20250706151958.png]]

![[Pasted image 20250706152205.png]]
### Multi-times NAR
![[Pasted image 20250706152720.png]]

#diffusion #NAR
![[Pasted image 20250706152743.png]]

![[Pasted image 20250706152945.png]]

![[Pasted image 20250706152935.png]]
## Summary
![[Pasted image 20250706153107.png]]
# Speculative Decoding
> “Speculative Decoding”常见释义为“推测解码”。它指的是一种解码方式，模型在生成文本等输出时，**基于已有的信息和概率推测，尝试预测后续可能的内容并进行解码输出**，以提高生成效率或探索多种可能的结果。例如在文本生成任务中，模型依据前文推测后续的单词或语句并进行解码生成，**不必按部就班地逐个生成**。 

## 预测token
![[Pasted image 20250706160241.png]]
## 犯错代价小
![[Pasted image 20250706160439.png]]

## 谁可以做预言家
![[Pasted image 20250706160825.png]]

![[Pasted image 20250706161018.png]]