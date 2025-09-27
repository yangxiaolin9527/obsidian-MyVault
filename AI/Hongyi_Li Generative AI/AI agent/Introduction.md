> 让AI完成多步骤、复杂的任务

![[Pasted image 20250708104544.png]]
![[Pasted image 20250708105029.png]]
## How about RL?
![[Pasted image 20250708105125.png]]
## Let LLM be Agent
![[Pasted image 20250708105235.png]]

### Just do what LLM can do
> 依靠LLM自身的一定程度的通用能力，将它当作Agent进行应用

![[Pasted image 20250708105547.png]]

### Advantage on RL-based agent
> 精细化的反馈信息，而不是人类手工设置reward（复杂的调参）

![[Pasted image 20250708110029.png]]

### More realistic interactions
![[Pasted image 20250708110557.png]]

# Key abilities of LLM-based AI agent
## Adjust behavior/output based on experience
![[Pasted image 20250708111505.png]]
### memory and RAG
#RAG #memory
![[Pasted image 20250708111639.png]]
#### memory important things
![[Pasted image 20250708112605.png]]
### positive feedback is better
![[Pasted image 20250708112100.png]]
## Use tools
### Function call
![[Pasted image 20250708112800.png]]

![[Pasted image 20250708113100.png]]

### select and make tools
> idea和RAG类似，将众多的agent tools descriptions放入memory中，构建tool selection module，根据observation从中选取适合的tool;
> 另一方面，LLM本身可以调用code tools,自行编写代码，构建简单的tools。

![[Pasted image 20250708113542.png]]

### Can we trust the tools
![[Pasted image 20250708121540.png]]

![[Pasted image 20250708121740.png]]

## Can AI agent make a plan
![[Pasted image 20250708122406.png]]

### benchmark
![[Pasted image 20250708122628.png]]

![[Pasted image 20250708122845.png]]

### world model
> AI agent的真实action操作（与真实世界的交互）代价过大，我们需要一个模拟的世界（Environment）

![[Pasted image 20250708123444.png]]
#### AI agent and reasoning
> 推理过程中，在虚拟world model中和environment进行交互，进行**搜索**

![[Pasted image 20250708124722.png]]
