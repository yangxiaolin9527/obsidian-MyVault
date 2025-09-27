> **ICML 2020**

* [ ] 理解理论部分，实验部分不看，重点：AID中用CG是为了解线性方程，用在paper问题描述部分。
# Abstract
We study a general class of bilevel problems, consisting in the minimization of an upper-level objective which depends on the solution to a parametric fixed-point equation. Important instances arising in machine learning include hyperparameter optimization, meta-learning, and certain graph and recurrent neural networks. Typically the gradient of the upper-level objective (hypergradient) is hard or even impossible to compute exactly, which has raised the interest in approximation methods. We investigate some popular approaches to compute the hypergradient, based on reverse mode iterative differentiation and approximate implicit differentiation. Under the hypothesis that the fixed point equation is defined by a contraction mapping, we present a unified analysis which allows for the first time to quantitatively compare these methods, providing explicit bounds for their iteration complexity. This analysis suggests a hierarchy in terms of computational efficiency among the above methods, with approximate implicit differentiation based on conjugate gradient performing best. We present an extensive experimental comparison among the methods which confirm the theoretical findings.

Translate:
我们研究了一种广泛应用的双层优化问题，这类问题的核心是找出最佳的上层目标值，而这个目标值取决于一种特殊的方程——参数化不动点方程的解。在机器学习领域，这种问题的具体应用包括超参数优化、元学习以及某些图神经网络和循环神经网络。通常情况下，要精确计算上层目标的梯度（也就是超梯度）是非常困难甚至不可能的，因此我们对近似计算方法产生了浓厚兴趣。我们研究了几种流行的计算超梯度的方法，这些方法主要是基于反向模式的迭代微分和一种称为近似隐式微分的技术。我们假设不动点方程是由一种特殊的映射——收缩映射来定义的，在这个假设下，我们首次进行了一项统一的分析，这使我们能够量化地比较这些方法，并为它们的迭代复杂度提供了具体的界限。我们的分析表明，在这些方法中，有着不同的计算效率层次，其中**基于共轭梯度的近似隐式微分方法表现最为出色**。我们还进行了一系列广泛的实验比较，证实了我们的理论发现。

