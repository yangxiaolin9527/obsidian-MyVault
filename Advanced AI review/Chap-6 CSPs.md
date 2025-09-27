> 与搜索问题不同，我们不关心`planning`，而是简单地判断某个`state`是否满足约束，而不用考虑如何到达该状态。

# Definition
1. **Variables** - CSPs possess a set of N variables X1;:::;XN that can each take on a single value from some defined set of values.
2. **Domain** - A set fx1;:::; xdg representing all possible values that a CSP variable can take on.
3. **Constraints** - Constraints define restrictions on the values of variables, potentially with regard to other variables.
# To Search
> `CSPs`属于`NP-hard`问题，为了更好地解决这类问题，常常将其形式化为搜索问题。

## 5要素
* **state**:为变量进行有效赋值（对n个变量一个个赋值，不妨设前$k$个赋值完成，此时的赋值会使前$k$个变量相关的约束条件都满足）
* **start state**:$k=0$,赋值变量集为`{}`
* **successor function**:在未赋值变量中挑选一个为第$k+1$个变量并对第$k+1$个变量赋值
* **goal test**:$k == n?$
* **cost function**:一般假设单步耗散为$1$
## 差别
* 扩展节点时可以任意选择一个未赋值的变量来扩展节点
* 不需要存储到达当前节点的路径，因此可以用回溯算法
# Constraint Graph
## Constraint classes
* Unary Constraints
* **Binary Constraints**
* Higher-order Constraints
## Graph

> 节点表示变量，可以赋值；边表示变量间的约束

![[Pasted image 20240108155518.png]]
# How to solve CSPs
## Basic Backtracking Thought
> 与搜索那样必须等全部变量都赋值结束再进行`goal test`不同，在赋值过程中，如果发现冲突现象，可以采用回溯算法回到某一节点，进行另外方向的探索。

![[Pasted image 20240108160120.png]]
## Filtering, Forward Checking
[`Arc3 Algorithm`](C:\Users\YangHL\Desktop\Advanced%20AI\CS188\n2-CSPs.pdf)
## Ordering, MRV LCV

>虽然`CSPs`中选取变量进行赋值的顺序不会影响最终的结果，但合适的顺序会大大提升解决问题的效率。实践中主要依据`MRV`和`LCV`两个原则确定`variable`及对应`domain`中`value`的选取顺序。

### MRV
> Variable Ordering: Minimum Remaining Values (MRV):
> **Choose the variable with the fewest legal left values in its domain**

**快速失败思想**:选最难的路（往往可以成功，就算失败，也不会浪费太多时间），所以需要选择一个`domain`中具有最少可选择`value`的`variable`。
### LCV
> Value Ordering: Least Constraining Value:
> **Given a choice of variable, choose the least constraining value**

一旦决定了艰难的路，我们尽可能尝试少的次数就能成功，所以需要选择一个对`graph`**影响最小**的`value`（例如通过`forward checking`排除一些`variable`的`value`）。
## Local search, Mini-Conflict

> 回溯搜索并不是解决约束满足问题的唯一算法。另一种被广泛使用的算法是`Locl Search`，它的思想幼稚简单，但却非常有用。局部搜索通过迭代改进来工作——从一些随机赋值开始，然后反复选择违反约束最多的变量，并将其重置为违反约束最少的值(一种称为`Mini-Conflict`启发式的策略)

**往往使用与约束很多或约束很弱的问题（如N-queen）**
![[Pasted image 20240108164324.png]]