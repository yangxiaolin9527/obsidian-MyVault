[自注意力机制](C:\Users\YangHL\Desktop\Computer%20Science\AI\DeepLearning\self_v7.pdf)
# 任务描述
## 输入
> **输入为不等长的向量序列**

![[Pasted image 20250616223425.png]]
### **NLP任务**
![[Pasted image 20250616223716.png]]
### **语音任务**
![[Pasted image 20250616223645.png]]
### **图**
![[Pasted image 20250616223813.png]]
## 输出
### sequence labeling
![[Pasted image 20250616224118.png]]
### one-label output
![[Pasted image 20250616224424.png]]
### seq2seq
![[Pasted image 20250616224530.png]]

# 自注意力机制
> Attention模块考虑整个sequence的信息
****## 考虑上下文信息（同级输入）
[自注意力机制](C:\Users\YangHL\Desktop\Computer%20Science\AI\DeepLearning\self-attention.xmind)
![[Pasted image 20250616225040.png]]
![[Pasted image 20250617105631.png]]
### 寻找相关性
![[Pasted image 20250617105801.png]]
### 计算相关性、加权输出

![[Pasted image 20250617152911.png]]

## 矩阵与并行计算
### 通过$W_q,W_k,W_v$（待学习的三个矩阵）与$I$ (input矩阵) 并行计算每个vector的q,k,v，分别得到矩阵$Q,K,V$
![[Pasted image 20250617153114.png]]
### 通过$Q,K$计算相关性矩阵$A$，并进行规范化得到$A'$
![[Pasted image 20250617153959.png]]
### 通过$A', V$计算输出$B$矩阵，每一个列向量为对应$a_i$的输出$b_i$
![[Pasted image 20250617154051.png]]
### 总结
![[Pasted image 20250617154151.png]]
# 多头注意力机制（MHA）
> 同级输出间，可能存在多种类型的上下文（相关性）

![[Pasted image 20250617154320.png]]

![[Pasted image 20250617154351.png]]

**增加一个可学习的$W_O$矩阵，对多头输出（concatenate后的矩阵）进行线性变换**
![[Pasted image 20250617154413.png]]
# Position Encoding
> 前文所述的注意力机制并没有考虑sequences中各vectors的位置，有时候，位置信息很重要

![[Pasted image 20250617160252.png]]

# Self-Attention 应用
$L^2$的复杂度
![[Pasted image 20250617160449.png]]
![[Pasted image 20250617160526.png]]
# Self-Attention v.s. CNN
> 全局感受野和局部感受野

![[Pasted image 20250617160630.png]]
![[Pasted image 20250617160721.png]]
# Self-Attention v.s. RNN
> Attention基本可以替代RNN，**并行计算**优势+**长距离上下文提取（q、k天涯若比邻）**优势

![[Pasted image 20250617160801.png]]
# Self-Attention v.s. GNN
> 图的邻接矩阵已经包含了相关度的信息（节省计算）

![[Pasted image 20250617161205.png]]

# Self-Attention与Transformer
Transformer中最常用模块
缺点：计算复杂度高($\mathcal{O}(L^2)$)，速度慢 