---
layout: post
title: Projection Matrices（理论）
tags: [科普]
---
#### 投影矩阵

* 关键词：投影矩阵

### 一、理论基础

> 一个向量(b)在另一个向量(a)上的投影(实际上就是寻找在a上离b最近的点)：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-12-Projection/1.png)

我们假设投影点是向量上的一点p，可以规定p=xa（x是某个数）。

如果我们把p看作是a的估计值，那么我们定义：e=b-p，称e为误差。

> e与p也就是a垂直，所以有:

![image]({{ site.baseurl }}/assets/img/blog/2018-04-12-Projection/2.png)

> 展开化简得到：

![image]({{ site.baseurl }}/assets/img/blog/2018-04-12-Projection/3.png)

* 如果改变b，那么p相对应改变，然而改变a，p无变化。

### 二、投影矩阵(projection matrix)



