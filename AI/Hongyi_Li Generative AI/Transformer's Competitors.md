# Introduction

**self-attention 取代 RNN**
![[Pasted image 20250711153455.png]]

## RNN-styles
![[Pasted image 20250711154112.png]]

### LSTM
> 让$f_A, f_B, f_C$与门（gate)有关

![[Pasted image 20250711154252.png]]
#### connection to the memory module in AI-Agent
#memory 
![[Pasted image 20250711154458.png]]

## RNNs and Attention
![[Pasted image 20250711155126.png]]

### Attention is all you need
#attention
==More parallelization to train, but longer context, more computation==

GPU并行计算，矩阵操作（QKV）
![[Pasted image 20250711155934.png]]

![[Pasted image 20250711160220.png]]

# parallelized RNN
## Linear attention
#linear_attention
![[Pasted image 20250711161303.png]]

![[Pasted image 20250711161354.png]]

### train作attention，inference作RNN
![[Pasted image 20250711161529.png]]

![[Pasted image 20250711161551.png]]

### Why not LA? No Softmax.
![[Pasted image 20250711165844.png]]

#### Add reflection
> 根据当前输入，更新过去的记忆权重

![[Pasted image 20250711170719.png]]

## DeltaNet and Titans
![[Pasted image 20250711171440.png]]

