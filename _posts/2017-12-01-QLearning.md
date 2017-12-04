---
layout: post
title: 增强学习
tags: [科普]
---
#### 在没有棋谱或者老师的情况下，怎样才能学会下棋呢？

可以通过尝试随机走棋，然后逐步建立对环境的一个预测模型，就能判断出走出某步后棋盘的形势，然后选择对己方最优的下棋方案。如果赢了，就意味着好的事情发生了；而输了，则意味着事情不妙。这类反馈称为回报。

增强学习的任务就是利用观察到的回报来学习针对某个环境的最优或者接近最优的策略。

在很多复杂领域中，增强学习是对程序进行训练以表现出高性能的唯一可行途径。

* 关键词： --

可以认为增强学习包括了人工智能的所有要素：一个智能体被置于一个环境中，并且必须学会在其间游刃有余。

### 一、智能体设计

#### 1.基于效用的智能体学习关于状态的效用函数，然后使用它选择使得结果的期望效用最大化的行为。

>基于效用的智能体还必须具备环境模型以便进行决策，因为它必须知道其行动将导致的状态。只有如此，它才能将效用函数应用于结果状态。

#### 2.Q学习智能体学习行为-价值函数，或称为Q函数，该函数提供给定状态下采取特定行为的期望效用。

>一个Q学习智能体可以比较各种可能选择的价值，而不必知道其选择会带来的后果，所以它不需要环境模型。另外，因为它们不知道其行动将引向何方，Q学习智能体无法前瞻，将看到这会严重限制它们的学习能力。

#### 3.反射型智能体学习一种策略，直接从状态映射到行为。

### 二、被动增强学习

在完全可观察环境下使用基于状态的表示的被动学习智能体的情形。在被动学习中，智能体的策略π是固定的，处于状态s，它总是执行行动$$π(s)$$。其目标是学习效用函数：$$U^π(s)$$

显然，被动学习的任务与策略评估（策略迭代算法的一部分）的任务相似。主要区别在于被动学习智能体对概率转移模型$$T(s,a,s')$$一无所知，并且也不知道状态s的回报函数$$R(s)$$。可以通过大量的在线试验来学习转移模型$$T(s,a,s')$$和回报函数$$R(s)$$

#### 1.直接状态估计

基本思想是：认为一个状态的效用是从该状态开始往后的期望总回报，而每次试验对于每个被访问到的状态提供了该效用值的一个样本。在进行无穷多次实验的极限情况下，样本平均值将收敛于真实期望值。因为采用会以正确的概率出现，所以此方法是可行的。

>进行多组试验，然后对出现的每个样本进行采样，并计算采样值的公式为：

$$Tsample_1=R(s,π(s),s_1')+\gammaV_i^π(s_1')$$

#### 2.基于模型的估计