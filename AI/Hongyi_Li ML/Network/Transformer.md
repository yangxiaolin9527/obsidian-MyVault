#Transformer
[Transformer](C:\Users\YangHL\Desktop\Computer%20Science\AI\DeepLearning\seq2seq_v9.pdf)
# seq2seq 任务
![[Pasted image 20250617115923.png]]

Text-to-Speech (TTS)
Speech-to-Speech
QA problems (NLP)
Syntactic Parsing
Multi-**label** Classification
Object Detection
# 力大砖飞
![[Pasted image 20250617162030.png]]

# Seq2Seq架构
![[Pasted image 20250617163208.png]] 
## Encoder
> Vectors->Vectors

### 简化设计
![[Pasted image 20250618134008.png]]

### Block in Transformer: residual connection and layer normalization

==注意layer normalization和batch normalization的区别==
![[Pasted image 20250618153721.png]]
## Decoder
### 自回归(autoregressive)
#autoregressive
以Speech Recognition为例

==输入的vectors越来越多（自己的输出作为上下文）==
![[Pasted image 20250618154720.png]]

#### 模块结构
![[Pasted image 20250618155036.png]]
##### mask
> self-attention计算输出$b_k$时，只能考虑$b_i, i=1,...,k-1$的上下文。**--输出是一个一个token输出的**。

![[Pasted image 20250618155420.png]]
![[Pasted image 20250618155601.png]]
##### STOP Token
![[Pasted image 20250618155821.png]]
### Non-Autoregressive
#NAR 
![[Pasted image 20250618160256.png]]

#多模态
在自然语言处理（NLP）任务中，NAT（Non-Autoregressive Transformer）通常比AT（Autoregressive Transformer）表现更差，主要原因是多模态性（Multi-modality）问题。
1. 什么是多模态性？
	**多模态性指的是目标序列可能存在多个有效的生成方式**。例如，在机器翻译任务中，给定一个源句子，目标语言可能有多种合法的翻译方式。这些翻译**在语义上是等价的，但在词序或表达方式上可能不同。**
2. AT的工作机制
	AT是自回归模型，它逐步生成目标序列的每一个词，每一步都依赖于之前生成的词。具体来说，AT的生成过程可以表示为：
$$
P(y) = \prod_{t=1}^{T} P(y_t | y_{<t}, x),
$$
	其中，$y_t$表示目标序列的第$t$个词，$y_{<t}$表示之前生成的词，$x$表示源序列。
	由于AT是逐步生成的，它能够通过条件概率逐步减少多模态性问题的影响。例如，如果在第$t$步已经生成了某些词，这些词会对后续的生成起到约束作用，从而减少目标序列的多样性。
3. NAT的工作机制
	NAT是非自回归模型，它试图一次性生成整个目标序列，而不是逐步生成。NAT的生成过程可以表示为：
$$
P(y) = \prod_{t=1}^{T} P(y_t | x),
$$
	其中，目标序列的每个词$y_t$仅依赖于源序列$x$，而不依赖于其他目标词。
	由于NAT缺乏目标词之间的依赖关系，它无法有效地处理多模态性问题。例如，NAT在生成目标序列时可能会同时考虑多种翻译方式，导致生成的序列质量下降，或者出现不一致的结果。
4. 多模态性对NAT的影响
	**多模态性问题会导致NAT模型难以学习到明确的目标分布**。具体来说，目标序列的多个可能翻译会使得NAT模型的训练目标变得模糊。例如，在训练过程中，模型可能会尝试同时优化多个翻译方式的概率分布，这会导致生成的序列质量下降。
	相比之下，**AT通过逐步生成的方式，可以逐步减少多模态性问题的影响**，从而生成更高质量的序列。
5. 总结
	NAT通常比AT表现更差的主要原因是多模态性问题。由于NAT一次性生成整个序列，缺乏目标词之间的依赖关系，它难以有效处理目标序列的多样性。而AT通过逐步生成的方式，可以更好地应对多模态性问题，从而生成更准确和一致的序列。
### Cross-Attention
#cross-attention
![[Pasted image 20250618160814.png]]

通过Masked MHA的输出计算$q_i$,通过Encoder的输出计算$k_i,v_i$，计算得到Cross-Attention的输出
![[Pasted image 20250618161052.png]]

# How to train Transformer
> Minimize the sum of Cross-Entropy

## Note
1. 输出sequences中的每一个vector都是一个分布，可以与one-hot编码的groud truth计算一个CE。
   ![[Pasted image 20250618162330.png]]
2. STOP Token也是要学习的。
3. Decoder的输入是groud truth（不包含STOP Token）
   ![[Pasted image 20250618162340.png]]
## Train Tips
### Copy Mechanism
![[Pasted image 20250618162657.png]]
### Guided Attention
![[Pasted image 20250618163313.png]]

### Beam Search
> “Beam Search”即“集束搜索”，是一种启发式图搜索算法。在搜索过程中，它每一步会保留一定数量（这个数量被称为集束宽度）的最优候选解，而不是像广度优先搜索那样扩展所有节点，或像贪婪搜索那样只保留一个最优解。这样**既避免了广度优先搜索的高空间复杂度，又比贪婪搜索更有可能找到全局最优解**。比如在自然语言处理的机器翻译任务中，对一个句子进行翻译时，每生成一个词，会从众多可能的词中选择得分最高的几个词作为候选继续生成后续词，直到生成完整的译文，这其中就可能用到集束搜索。 

![[Pasted image 20250618163433.png]]

**任务特定和温度设置（随机性， noise is sometimes useful）**
![[Pasted image 20250618163726.png]]

### Optimizing Evaluation Metrics
> 训练时CE作为Loss，但是不同的任务可能有更加精确的评分标准。例如翻译任务中的BLEU score。--->Mismatch
> 能否训练时也采用任务特定的metric?
> 	- metric可微-->可以使用梯度下降
> 	- metric不可微-->仍然将CE作为Loss / RL进行优化

![[Pasted image 20250618164059.png]]

### Exposure Bias--->Scheduled Sampling
> 如果训练时只接触非常正确、完美的标注样本，测试时某一个vector出错时，会带偏后续的输出。
> 在机器学习尤其是序列生成模型（如语言模型、图像生成模型等）的训练过程中，曝光偏差指的是训练时模型输入真实数据，但在实际应用时却依据自身生成的数据作为后续输入，这种训练与实际应用场景输入数据的差异，会导致模型性能在实际使用时出现偏差。例如在训练一个文本生成模型时，**训练过程中每个时刻都基于真实的前文生成下一个词，但在实际应用时，模型只能依赖自己之前生成的词来生成后续内容，这就可能导致模型在实际应用时产生与训练预期不同的结果。** 
> “Scheduled Sampling”是作为解决曝光偏差（Exposure Bias）问题的一种方法。是一种**按照特定计划，动态调整训练过程中使用真实标签和模型生成结果作为输入的策略**，以减少这种差异。例如在训练早期更多使用真实标签，**随着训练推进，逐渐增加使用模型生成结果作为后续输入的比例，从而使模型训练与实际应用时的输入情况更接近**。 

![[Pasted image 20250618164510.png]]