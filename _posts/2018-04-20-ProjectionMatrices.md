---
layout: post
title: Projection Matrices（理论）
tags: [科普]
---
#### 投影矩阵

* 关键词：投影矩阵

### 一、理论基础

> 一个向量(b)在另一个向量(a)上的投影(实际上就是寻找在a上离b最近的点)：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-20-Projection/0.png)

我们假设投影点是向量上的一点p，可以规定 p = xa（x是某个数）。

如果我们把p看作是a的估计值，那么我们定义：e = b - p ，称e为误差。

> e与p也就是a垂直，所以有:

![image]({{ site.baseurl }}/assets/img/blog/2018-04-20-Projection/2.png)

> 展开化简得到：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-20-Projection/3.png)

* 如果改变b，那么p相对应改变，然而改变a，p无变化。

### 二、投影矩阵(projection matrix)

假设一个平面基(basis)是$$a_{1}$$,$$a_{2}$$，那么矩阵 A 的列空间就是该平面。

假设一个不在该平面上的向量 b ，在该平面上的投影是 p 。

任务就是找到合适的 x ，使得 p = Ax 。

> e 与该平面垂直，故：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-20-Projection/4.png)

===========================================（进入主题分割线）===================================================

#### 投影矩阵(projection matrix)：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-20-Projection/5.png)

> 我们最需要关注的是投影矩阵的两个性质：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-20-Projection/6.png)

#### (待更新)

最小二乘法的解与矩阵投影时对变量求解的目标是一致的。

#### 原文资料

[最小二乘法与投影](https://blog.csdn.net/qq_29573053/article/details/79159752)


