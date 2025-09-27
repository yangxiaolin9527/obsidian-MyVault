# What does one neuron do?
#embedding 

![[Pasted image 20250710144016.png]]

![[Pasted image 20250710145033.png]]

1. 确定神经元和某个现象的相关性
2. 移除该神经元，观察现象（输出置0，或者置为均值）
3. 改变神经元的激活程度(神经元的激活并不一定要其对应输出>0，只要>均值即可在广义上认为被激活了)，观察现象

==实际上，并不容易解释单个神经元的功能==
# What does one layer's nerve do？
## 功能向量假说
> 可以从高维线性代数的角度理解，假设embedding向量维度为N，那么所有可能的embedding所张成的高维空间维度为N,但是token代表的语义数量P远大于N。**虽然语义之间的关系普遍是非线性的， 但是存在一些特定的语义概念（方向）在LLM语义空间中是近似线性编码的**，可以找到一些功能向量，沿着该向量方向进行移动，可以稳定地改变原有embedding表示的语义。例如，$V_{\text{国王}}-V_{\text{男人}}+V_{\text{女人}}\approx V_{\text{王后}}$。

![[Pasted image 20250710150322.png]]

### 支撑实验
![[Pasted image 20250710150747.png]]

![[Pasted image 20250710151031.png]]
![[Pasted image 20250710151042.png]]
![[Pasted image 20250710151053.png]]

#representation_engineering #activation_engineering
![[Pasted image 20250710151305.png]]
### In-context Vector
![[Pasted image 20250710151923.png]]

![[Pasted image 20250710151935.png]]
# What does a sparm of nerve do?
## Model of the LLM
> 进一步的抽象

![[Pasted image 20250710155725.png]]

#pruning
![[Pasted image 20250710160752.png]]

# Let LLM explain something
## Logit lens
> 抽象地理解，可以认为原始语义vector经过residual stream直接到达unembedding之前的logit。其间，transformer模块会不断将信息添加到residual stream中。直到最后，logit经过unembedding、softmax得到大小为N（词编码空间维度）的分布，代表经过上下文推理后得到的输出概率分布。
> 那么，能否在residual stream中，就将representation进行类似的unembedding、softmax操作，同样得到一个分布，通过观察该分布，可以尝试理解模型在生成内容的决策过程。

#logit_lens #resdiual_stream
![[Pasted image 20250710164923.png]]

![[Pasted image 20250710170519.png]]

### What are plugged into the residual stream
> 对每一个层的神经元，有两种理解方式
> - 单个neuron对input $x$做weighted sum。（$W$看作行向量排列，每个行向量和$x$做inner product)
> - $x$的每个分量，为各个neuron贡献一定权重的信息（$W$看作列向量排列，输出是列向量的以$x$为权重的线性组合)

![[Pasted image 20250710171621.png]]

==根据第2种理解，可以更进一步地，对$v_i$进行unembedding, softmax操作，观察其分布，尝试理
解其语义。==

## Patchscopes
#patchscopes
![[Pasted image 20250710173533.png]]

**和左边准备的输入有关**
![[Pasted image 20250710173632.png]]

### Multi-Hop Queries(Reasoning)
> 某种程度上，和reasoning的“Testing Time Scaling"技巧有异曲同工之妙。

#reasoning 
![[Pasted image 20250710174116.png]]

![[Pasted image 20250710174144.png]]
