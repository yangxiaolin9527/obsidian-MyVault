# Submission Instruction
1. 匿名提交，使用Adobe清洗PDF元数据等敏感信息
2. 匿名提交阶段只需要 PDF
   包括不超过7页的正文与2页的参考文献，附录单独在openreview中提交。
4. group figure禁止使用`minipage`语句---参考AAAI24文章重新绘图(matplotlib重新画 / `subcaption`包)
5. 表格的标题和标号必须位于表格的下方，插图位于顶部才可以横跨两栏
6. Abstract+Introduction: 包含contributions，1.5~2 page

# Rebuttal

## Rebuttal Instruction
1. 时间：UTC时间 11.4-11.8 具体的时间点没有发布
2. 平台：OpenReview
3. Review个数：一般来说，第一轮2个，第二轮再增加2个。
4. 具体要求：往年在cmt平台，是所有的response都在一页；今年更换成OpenReview平台，官网上没有明确具体的格式要求，应该会在第二轮邮件里说明。
## Review QUUZ
1. C1.1: meta learning methods 是否需要替换成 meta-learning methods
2. C1.2: affect 译作影响时通常作为动词，替换成 effect
3. C2: 
   * increasing 改为 to increase
   * of the order of 改为 of the order
4. inner/outer-loop iterations 的单复数问题
   原文表述：the number of inner-loop iterations / inner-loop iteration number
5. C3: plan to added 改为 plan to add

## Review CwWv
1. Q1.1 
   * because coupled subroutine 改为 because **a/the** coupled subroutine；
   * decays 改为 decay
2. Q1.2: 
   * reduces memory cost 改为 reduce memory cost
   * meta gradient 改为 meta-gradients
   * meta-gradient aggregation 改为 meta-gradient collection
   * despite the inner-loop iteration increases 改为 despite the inner-loop iteration increasing
3. Q3: with complexity of 改为 with a complexity of 
4. Q4: 
   * 表格数据修改；将迭代次数由2000修改为4000 和 Reviewer 5yK9中实验结果对应
   ```markdown
| Algorithm\Dataset  | CIFAR-FS\* | tieredImageNet\* |
| ------------------ | ---------- | ---------------- |
| FOMAML             | 67.27      | 65.07            |
| MetaOptNet-SVM     | 78.36      | 70.03            |
| R2D2               | 77.41      | 68.78            |
| Algorithm 1 (ours) | 69.12      | 67.99            |
   ```
   * Meta-Opt-Net 修改为 MetaOptNet-SVM 或者 MetaOptNet 与原文方法描述相对应

## Review QiJk
1. W1: and further **ensures** 改为 and further **ensure**
2. W2: where data are **proceed** sequentially within and across tasks 改为 processed 

## Review 5yk9

1. W2 
   * 对MAML表现效果不佳的解释，收敛速度的缓慢，文献改为
   `Ji, Kaiyi, et al. "Convergence of meta-learning with task-specific adaptation over partial parameters." Advances in Neural Information Processing Systems 33 (2020): 11490-11500.`
   * its **accuacy** reach 改为 its ==accuracy== **reaches**
2. W3
	* iMAML，Algorithm in [1] 在miniImageNet 5-1和5-5上的对比表格
	```markdown
|Algorithm|5-way 1-shot\*|5-way 5-shot\*|
|-|-|-|
|iMAML| 47.22|64.07|
|Algorithm in [1]|48.13|64.24|
|Algorithm 1(ours)| 48.87|65.87|
```
	* 迭代次数修改为 4,000，和Q2对应
3. SEC
   * represent an  "optimal initial 中间多占用了一个空格字符
   * model parameter 改为 model parameters
   * our meta learning framework, which can be viewed as bilevel optimization 改为 our **meta-learning framework**, which can be viewed as bilevel **optimization/programming**
   * e.g., **hyperparmeter** optimization 改为 e.g., **hyperparameter** optimization
   * meta parameter and base parameter 改为 meta parameter**s** and base parameter**s**
4. Q1
   * ``MNIST'' 修改为匹配Markdown的格式 ''MNIST''
5. Q2 
   增加迭代数至4000步后，算法在不同数据集上的对比（注意表格间值的对应）
   ```markdown
|Algorithm|CIFAR-FS\*|FC100\*|miniImageNet\*|tieredImageNet\*|
|-|-|-|-|-|
|MAML|68.12|48.20|62.11|64.74|
|ANIL|67.87|48.35|63.62|65.49|
|ITD-BiO|67.99|48.31|61.07|63.37|
|Algorithm 1 (ours)|69.12|49.14|65.87|67.99|
```

![[Pasted image 20241107200657.png]]

MAML 原文性能：miniImageNet：63% 
	 其他文章补充 tieredImageNet 70% CIFAR-FS 70% FC100 -
ANIL 原文性能：miniImageNet 61%
R2D2 原文性能：miniImageNet 68% tieredImageNet - CIFAR-FS 78% FC100 -
MetaOptNet 原文性能 miniImageNet 68 tieredImageNet 71% CIFAR-FS - FC100 -

iMAML 原文性能 5-5：- 5-1：48%
FOMAML 原文性能 5-5 63 5-1：48%
[1] 实验性能：5-5：:62% 5-1 46%
Algorithm 1 实验性能：5-5:63% 5-1 45%

## Author SPC Confidential Comment
1. The review from Reviewer #cwWv is **inaccuracy** 改为 ... **inaccurate**
2. requires less **walkclock** time to reach a given accuracy 改为 ... **wallclock** time ...
3. The reviewer's **focusing** on，focus 本身可做名词
4. is **unfiar** 改为 is **unfair**
5. mention that most **of** meta-learning algorithms **is** hard 改为 mention that most meta-learning algorithms **are** hard
 ==不确定：==
1. express our concerns on reviews #cwWv 改为 ... on reviews of/from Reviewer #cwWv 或 ... on Reviewer #cwWv 
2. we believe that Review #cwWv is clearly wrong and unreasonable 改为 ... that Reviewer #cwWv is ... 或者 ... that the reviews of/from Reviewer #cwWv are ... 

# Accept
## Camera-Ready
### 提交内容
1. 一个`*.zip`文件，以第一作者的姓命名，不超过10MB，包括如下：
	* [ ] PDF文件
	* [ ] 单个`*.tex`的Latex源文件（不能使用`input`命令）
	* [ ] `*.bib`文件（因为要提交，是否将引用名一起修改）
	* [ ] 论文中实际使用的图形文件
	* [ ] 编译过程中Latex生成的文件
2. copyright
   * title: `Memory-reduced Meta-learning with Guaranteed Convergence`
   * publication...: `Proceedings of the 39th AAAl Conference on Artificial Intelligence (AAAl-25)`
   * Author's name(s): Honglin Yang, Ji Ma, Xiao Yu
   * contact Author 签名
3. Appendix原则上无需提交，如果需要，可以**挂载arxiv上**，并引用
### 版权与样式
* [ ] Latex编译方式：pdfLatex
* [ ] 使用样式模版`aaai25.sty`和`aaai25.bst`
* [ ] You must complete, sign, and return by the deadline the AAAI copyright form (unless directed by AAAI Press to use the AAAI Distribution License instead).
    `C:\Users\YangHL\Desktop\AuthorKit25\Copyright`中包含了PDF版本的版权表，需要包含有效签名
* [ ] 论文第一页底部需要包含AAAI版权声明
* [ ] 支付页面所需要的费用？
### 格式要求
* [ ] 所有字体包括图形嵌入PDF文件
* [ ] 不能使用Type 3字体
* [ ] 不得更改标题、插图、标题、副标题上下方的间距
* [ ] 不得更改字体大小
* [ ] 不得更改文本行距
* [ ] 标题必须遵循大小写规则（首字母大写）
* [ ] 不得使用非罗马字母的字体
* [ ] AAAI两列格式
* [ ] source与PDF匹配
* [ ] source和PDF不得包含嵌入链接（`hyperref`和`navigator`包）
* [ ] source和PDF不得包含页码、页眉、页脚
* [ ] source由单个文件组成
* [ ] 图形在Latex之外调整大小（不得使用`clip`,`trim`包）
* [ ] 链接添加
    ```latex
    \begin{links} \link{Datasets}{https://aaai.org/example/datasets} \link{Code}{https://aaai.org/example/guidelines} \end{links} \link{Extended version}{https://aaai.org/example}
```  
* [ ] 不得使用分页符，references直接放在正文后，不得中断。
* [ ] 标题大小写格式需要满足要求：[在先格式检查转换网站](https://titlecaseconverter.com/)
    > **Is it possible to update the paper title?**
    > Yes, contact us at [proceedings-questions@aaai.org](mailto:proceedings-questions@aaai.org) and we will process the change. Do not forget to tell us your paper number!

### Check
* [x] Author
* [x] Acknowledge, grant number
* [ ] PDF meta data
## Rebuttal内容更改
### QUUZ
C1. `Subroutine 1`是否为估计HVP的标准方法？在收敛分析中是否考虑了`Subroutine 1`带来的估计误差。
C2. 证明定理1和定理2时所遇到的主要技术挑战
C3. 实验结果仅关注图像任务，增加其他类型任务的结果可能会提供对算法在不同领域的泛化能力的全面评估
* [ ] 是否需要补充说明 

### cwWv
Q1. 方法缺乏新颖性，并且算法在实际运用中并不快（因为需要CG步骤去计算HVP）
Q2. 认为需要报告每一步迭代所需的时间
Q3. 收敛分析
Q4. 实验缺少了其他的元学习算法
* [ ] 是否需要补充实验结果
### Q1jk
W1. 认为应该增加对文献`Online-Within-Online Meta-Learning`的讨论
```markdown
我们研究在完全在线的元学习环境中学习一系列任务的问题。目标是利用任务之间的相似性，逐步调整一个内部在线算法，以在这些任务上获得较低的平均累积误差。我们专注于一类基于参数化变体的在线镜像下降（Mirror Descent）的内部算法。内部算法通过一个在线镜像下降元算法逐步调整，使用相应任务内的最小正则化经验风险作为元损失。为了保持过程完全在线，我们通过在线内部算法来近似元次梯度。
对近似误差的上限允许我们推导出所提方法的累积误差界。我们的分析也可以通过在线到批处理的论证转换为统计设置。我们实例化了该框架的两个例子，其中元参数是一个公共偏置向量或特征映射。最后，初步的数值实验验证了我们的理论发现。
```
W2. 关于理论结果，`算法1`保证了结果随外循环次数`T`呈次线性收敛，收敛误差随采样批次小次线性衰减
### 5yk9
W1. 引用`[1] Lorraine, J., Vicol, P., & Duvenaud, D. (2020, June). Optimizing millions of hyperparameters by implicit differentiation. AISTATS2020.`，并包含类似`[1]`中`图2`的示意图
```markdown
我们提出了一种经济高效的基于梯度的超参数优化算法，该算法结合了隐函数定理（IFT）和高效的逆Hessian矩阵近似。我们展示了IFT与通过优化进行微分之间关系的研究结果，这为我们的算法提供了理论支持。我们使用该方法来训练具有数百万权重和数百万超参数的现代网络架构。
具体来说，我们学习了一个数据增强网络——在这个网络中，每个权重都是为验证性能而调整的超参数，该网络输出增强的训练样本；我们还学习了一个精简的数据集，其中每个数据点的每个特征都是一个超参数；此外，我们调整了数百万个正则化超参数。使用我们的方法同时调整权重和超参数，其在内存和计算上的成本仅比标准训练高出几倍。
```
![[Pasted image 20241216161740.png]]
W2. 迭代次数不足，增加迭代次数
W3. 增加对`[1]`, `imaml`等算法的对比


SEC. ![[Pasted image 20241216151408.png]]
![[Pasted image 20241216151436.png]]

* [x] 按照要求和提交指南修改 
Q1. 用该算法优化学习率或者优化器
Q2. 在实验中给出更多的迭代次数

### 更改内容
1. 语法Fig. -> Figure
2. 8分PC的launch Model，strong
3. bib中arXiv引用格式
4. 标题更改（todo）
5. 增加引用(online)



# 流程
## 一、准备arXiv版本
1. 标题更改：发邮件
2. 代码链接
3. 附录内容: 摘要后添加extend version[link url]-》超出页数
4. 文献增加`denevi2019online`?
5. Check内容
## 二、AAAI版本中引用arXiv

## 三、提交PDF、tex文件、copyright
* [x] 打印并签名copyright
* [x] 网站上提交PDF、source文件、copyright
## 四、注册
### 学生注册

## 五、缺席邮件
to aaaireg@aaai.org
Dear AAAI Organizing Committee Team,
Happy New Year! I hope this email finds you well. We are delighted that our paper, "Memory-Reduced Meta-Learning with Guaranteed Convergence" (ID: 10074), has been accepted. We have completed the registration for the conference and submitted the final version of the paper.
Unfortunately, due to visa issues, we are unable to present the paper in person. We would like to apply for presenting virtually. Could you please confirm if this is feasible and if it would affect the inclusion of our paper in the conference proceedings?
Thank you very much for your help. 
Best regards, 
Honglin Yang, Ji Ma, and Xiao Yu

![[response.jpg]]


## 六、Feedback
1. PDF更改
2. source更改
3. arXiv同步

![[Pasted image 20250106103825.png]]
* [x] 1.1 Contributions 每段第一行必须缩进，标题后除外。
  Add: Our contributions can be summarized as follows.
  ![[Pasted image 20250106103347.png]]
* [x] Figure 1,2中字体太小（最小不低于7pt），若无关紧要，可以删除
  PDF中字体大小没有找到好的确定方法。正文大小字体为10pt，截图后按比例缩放后确定差不多大小
	1. Figure 1:
	   **Before**: ![[Pasted image 20250106104903.png]]
	   **After**: ![[Pasted image 20250106104919.png]]
	2. Figure 2:
	   **Before**:![[Pasted image 20250106104828.png]]
       **After**:![[Pasted image 20250106104809.png]]
* [x] Metadata中Affiliations与文中信息对应
  **Before**:
    1. Honglin Yang
       Xiamen University
    2. Ji Ma
       Xiamen University
    3. Xiao Xu
       Xiamen University
  **After**:
    1. Honglin Yang
       Department of Automation, Xiamen University
       Key Laboratory of Multimedia Trusted Perception and Efficient Computing, Ministry of Education of China
    2. Ji Ma
       Department of Automation, Xiamen University
       Key Laboratory of Multimedia Trusted Perception and Efficient Computing, Ministry of Education of China
    3. Xiao Yu
       Institute of Artificial Intelligence, Xiamen University
       Key Laboratory of Multimedia Trusted Perception and Efficient Computing, Ministry of Education of China
* [x] Metadata中摘要信息不能使用任何Latex命令，需要为纯文本
    ```tex
    The optimization-based meta-learning approach is gaining increased traction because of its unique ability to quickly adapt to a new task using only small amounts of data. However, existing optimization-based meta-learning approaches, such as MAML, ANIL and their variants, generally employ backpropagation for upper-level gradient estimation, which requires using historical lower-level parameters/gradients and thus increases computational and memory overhead in each iteration. In this paper, we propose a meta-learning algorithm that can avoid using historical parameters/gradients and significantly reduce memory costs in each iteration compared to existing optimization-based meta-learning approaches. In addition to memory reduction, we prove that our proposed algorithm converges sublinearly with the iteration number of upper-level optimization, and the convergence error decays sublinearly with the batch size of sampled tasks. In the specific case in terms of deterministic meta-learning, we also prove that our proposed algorithm converges to an exact solution. Moreover, we quantify the computational complexity of the algorithm, which matches existing convergence results on meta-learning even without using any historical parameters/gradients. Experimental results on meta-learning benchmarks confirm the efficacy of our proposed algorithm.
```

## 七、Poster
size <= $40*40$
