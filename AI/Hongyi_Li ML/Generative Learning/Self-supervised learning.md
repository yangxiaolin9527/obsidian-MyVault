> large mode, need big (labelled) data which is expensive.

# 概览
#self-supervised-learning
> 输入自身的一部分作为label

![[Pasted image 20250622151454.png]]
# BERT
> 填词游戏
> **encoder-only**

## Masking
![[Pasted image 20250622151940.png]]

## Next Sentence Predition
![[Pasted image 20250622152520.png]]

## Pre-train and fine-tune
> Pre-train: a lot unsupervised label
> fine-tune: supervised

![[Pasted image 20250622152615.png]]

### \[CLS] and \[SEP]
> 在自然语言处理（NLP）中，特别是基于Transformer架构的模型（如BERT），`[CLS]` 和`[SEP]` 是两种特殊的标记（token），它们在输入序列中有特定的作用。

#### `[CLS]` 标记
`[CLS]` 是一个特殊的标记，通常被添加到输入序列的开头。它的主要作用是**为整个输入序列生成一个全局表示**。换句话说，`[CLS]` 标记的隐藏状态（hidden state）可以被用作整个输入序列的语义表示。
例如，对于一个输入句子：
```
"我喜欢学习机器学习。"
```
在输入到模型之前，序列会被处理成：
```
[CLS] 我 喜欢 学习 机器 学习 。 [SEP]
```

经过Transformer模型的编码后，`[CLS]` 标记**对应的隐藏状态向量可以被用作分类任务的输入**。例如，在文本分类任务中，假设隐藏状态为 $\mathbf{h}_{\text{[CLS]}}$，则分类器通常会使用如下公式：
$$
\hat{y} = \text{softmax}(\mathbf{W} \cdot \mathbf{h}_{\text{[CLS]}} + \mathbf{b}),
$$
其中 $\mathbf{W}$ 和 $\mathbf{b}$ 是分类器的参数。
#### `[SEP]` 标记
`[SEP]` 是另一个特殊的标记，通常用于分隔两个句子或标记输入序列的结束。
##### 单句子输入
对于单句子输入，`[SEP]` 标记通常放在句子的末尾，用来**表示序列的结束**。例如：
```
[CLS] 我 喜欢 学习 机器 学习 。 [SEP]
```
##### 两个句子输入
对于两个句子的输入，`[SEP]` 标记用于**分隔这两个句子**。例如：
```
句子1: "我喜欢学习机器学习。"
句子2: "深度学习很有趣。"
```
在输入到模型之前，序列会被处理成：
```
[CLS] 我 喜欢 学习 机器 学习 。 [SEP] 深度 学习 很 有趣 。 [SEP]
```

在这种情况下，模型可以**通过注意力机制（self-attention）同时关注两个句子之间的关系**。例如，在句子对分类任务（如自然语言推理任务）中，模型会利用两个 `[SEP]` 标记分隔的句子来判断它们之间的关系（如“蕴含”或“矛盾”）。

#### 总结
- `[CLS]`：表示序列的开始，通常用于生成全局语义表示。
- `[SEP]`：用于分隔句子或标记序列的结束。
这两个标记是BERT等模型中处理输入序列的重要组成部分，帮助模型理解句子的结构和关系。
## GLUE in NLP
> evaluation model's performance in NLP

![[Pasted image 20250622152746.png]]

## How to use BERT
### Case-1
![[Pasted image 20250622153211.png]]

![[Pasted image 20250622153405.png]]

### Case-2
![[Pasted image 20250622153636.png]]
### Case-3
![[Pasted image 20250622153759.png]]
![[Pasted image 20250622153932.png]]
### Case-4
![[Pasted image 20250622154546.png]]

#### start
![[Pasted image 20250622154951.png]]
#### end
![[Pasted image 20250622155008.png]]

## Hard to train from scratch
![[Pasted image 20250622155628.png]]

## Pre-train a seq2seq model
> Corrupt and reconstruct

![[Pasted image 20250622160039.png]]

![[Pasted image 20250622160246.png]]

## Why BERT works

### contextualized word embedding
![[Pasted image 20250622160401.png]]

![[Pasted image 20250622165143.png]]

## Multi-lingual BERT
![[Pasted image 20250622170023.png]]

![[Pasted image 20250622170100.png]]

![[Pasted image 20250622170204.png]]

### hard to train
![[Pasted image 20250622170419.png]]

### token-level translation
![[Pasted image 20250622170638.png]]
![[Pasted image 20250622170733.png]]
# GPT
> Generative Pre-train
> 接龙游戏

## 概览
> **decoder-only**

![[Pasted image 20250622171119.png]]

## How to use GPT
### In-context learning
> in-context learning 本身没有learning(梯度下降过程)

![[Pasted image 20250622171504.png]]

# Other

![[Pasted image 20250622171811.png]]

![[Pasted image 20250622171956.png]]