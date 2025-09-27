# 0210
## 1. log_interval 问题
1. 主要问题：本质是在当前的损失记录方式下，相邻两个epoch间`tot_loss`的处理。
	==声明`tol_loss`为实例变量，在`log`子线程重置==
2. 次要问题：当前`idx=0`时的处理，无法完全记录数据
	* way1: 针对`run_iteration==0`的`tot_loss`单独处理，`loss*=log_interval`
	* way2: 设置`run_iteration`从1开始
## 2. Algorithm4 调试
1. 运行速度优化
	* `@cache for generate_Ei()`
	* `deepcopy -> copy`
	* 尽可能避免`to(device)`
	* * `total_acc += (predicted_label.argmax(1) == labels).sum().item()`
	* 尽可能避免`clone().detach()`