#HyperGradient
# Formulation
![[object function of bilevel optimization.png]]

可以看出，在优化上层函数时，涉及到如下的梯度计算：
$$\nabla \Phi ( x ) = \frac { \partial f ( x , y ^ { * } ( x ) ) } { \partial x } $$
但是，往往无法直接求出$y^{*}(x)$,因为其还依赖于上层的$x$。
**How to solve the hypergradient?**
# Major Methods
* Tow major classes methods
	* **Approximate implicit differentiation** (AID)
		$$\nabla \Phi ( x ) = \nabla _ { x } f ( x , y ^ { * } ( x ) ) - \nabla _ { x } \nabla _ { y } g ( x , y ^ { * } ( x ) ) \left[ \nabla _ { y } ^ { 2 } g ( x , y ^ { * } ( x ) ) \right] ^ { - 1 } \nabla _ { y } f ( x , y ^ { * } ( x ) ) $$
		correspond to the Algorithm 1 in paper：
		![[Hypergradient in Algorithm1.png]]
		**Note**：different between $\nabla \Phi ( x )$ or $\frac { \partial f ( x , y ^ { * } ( x ) ) } { \partial x }$ and $\nabla _ { x } f ( x , y ^ { * } ( x ) )$
			See [chatGpt 4 Q&A](https://chat.openai.com/c/1b30a60b-7c82-4945-a492-b8fbc59c5756)
	* **Iterative differentiation** (ITD)
## Different between two major methods

> 共同假设：inner loop迭代出的$y^{D}_{k}$接近真实的最优值$y^{*}(x_{k})$。

### AID

真实的`HyperGradient`计算公式：
$$
\nabla \Phi(x_k) =  \nabla_x f(x_k,y^*(x_k)) -\nabla_x \nabla_y g(x_k,y^*(x_k)) v_k^{*}
$$
其中$v^{*}_{k}$是线性方程$\nabla_y^2 g(x_k,y^*(x_k))v= \nabla_y f(x_k,y^*(x_k))$的解。

`AID`使用$y^{D}_{k} \approx y^{*}_{k}$,$v^{N}_{k} \approx v^{*}_{k}$来估计。
需要求解`Hessian-Vector Product`解线性方程，以及求解`Jacobian-Vector-Product`参与进行计算。-----各种自动微分工具，方便计算。
### ITD

在`inner loop`使用`GD`迭代出$y^{D}_{k}$时，已经考虑了$y_{k}$对$x_{k}$的依赖，计算图中保存了相关链式信息，直接对`inner problem`进行误差反向传播操作，计算梯度。

### Summary
* `AID`计算效率更高，在数值上更加稳定。特别是当内部问题非常复杂，长距离反向链导致的梯度问题。
* `ITD`更加准确。方法的实质在于通过反向传播捕捉下层优化解对上层参数的依赖，它考虑了完整的优化轨迹，因此可以更准确。
# Implements
[hypertorch incorporates the calculation](https://github.com/prolearner/hypertorch/tree/master)
[Hypertorch-README.md](C:\Users\YangHL\Desktop\LDP\stocBIO\stocBiO\hypertorch\hypertorch_README.pdf)
