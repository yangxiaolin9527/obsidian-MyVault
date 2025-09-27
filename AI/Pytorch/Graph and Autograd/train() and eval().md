# BatchNorm and Dropout
#eval #batchnorm #dropout 
## train()
1. Dropout启用：在`forward`期间，随机丢弃神经元，减小过拟合
2. BatchNorm：在训练的`Iteration`期间，计算当前`mini-batch`数据的均值和方差，并使用`EMA(移动指数平均)`方式更新当前缓冲区`buffer`内的`running_mean`和`running_var`。
## eval()
1. Dropout禁用：在测试模式下，不进行`backward`，无需进行正则化处理；此外也为推理/验证提供确定性
2. BatchNorm：使用`buffer`内的`running_mean`和`running_var`，也即训练期间累积的值，**反应的是整个训练集的统计数据**而非测试时使用`mini-batch`的

# model.parameters() and model.state_dict()
#state_dict #parameters
## model.parameters()
==返回可学习参数(require_grad=True)的generator==，包括`weight`和`bias`。
通常如下使用：
```python
# 1
for param in model.parameters()
	pass
# 2
_ = list(model.parameters())
```

## model.state_dict()
==返回一个OrderredDict，除了具有可学习的**parameters**之外，还具有模型跟踪的不可学习的**buffer，通常包括`running_mean`, `running_var`, `num_batches_tracked`**==
通常如下使用：
```python
state_dict = model.state_dict()
model.load_state_dict(state_dict)
```
# Buffer
#buffer
## What
存储在模型中的`Tensor`，但是不视为可学习的`parameters`
## Where
1. BatchNorm：`running_mean`,`running_var`,`num_batches_tracked`,初始值分别为0,1,0
2. Custom buffers: 在模型定义中通过`self.register_buffer('name', tensor)`注册
## Why
1. BatchNorm: 训练期间跟踪`mini-batch`数据中的统计信息，在之后的训练和测试时使得数据的分布更加稳定，从而让神经网络的训练更加高效
2. Others: 存储其他不可学习的持久化`Tensor`,例如预计算的`masks`
## How
1. No Use Buffers:
	* 初始化新模型，从头训练
	* 模型中无`BatchNorm`
2. Use Buffers:
	* 需要使用现有模型存储的`buffer`信息
	  ```python
		# 1
		copy.deepcopy(model)	

		# 2
		model.load_state_dict()
		
		# 3
		for key in model.state_dict():
			if any(k in keys for k in ['weight', 'bias', 'running_mean', 'running_var']):
				...
		```