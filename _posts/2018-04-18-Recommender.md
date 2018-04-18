---
layout: post
title: 推荐系统（全面总结）
tags: [科普]
---
#### 个性化推荐系统应用--电子商务-电影和视频-音乐-社交-阅读-基于位置的服务-邮件-广告

* 关键词：推荐系统 -- 算法总结

### 一、推荐系统测评及实验方法

#### 评测指标

1. 用户满意度

我们可以用点击率、用户停留时间和转化率等指标度量用户的满意度。

2. 预测准确度

> 评分预测(RMSE和MAE)

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/3.png)

> 用python代码求RMSE和MAE：

```python
   //records[i] = [用户(u),物品(i),实际评分(rui),预测评分(pui)]

   def RMSE(records):
        return math.sqrt(sum([(rui-pui)*(rui-pui) for u,i,rui,pui in records]) / float(len(records)))

   def MAE(records):
   return sum([abs(rui-pui) for u,i,rui,pui in records]) / float(len(records))
```

RMSE加大了对预测不准的用户物品评分的惩罚（平方项的惩罚），因而对系统的评测更加苛刻。

如果评分系统是基于整数建立的（即用户给的评分都是整数），那么对预测结果取整会降低MAE的误差。

> TopN推荐（个性化的推荐列表）

> 准确率(precision)/召回率(recall)，R(u)是用户在训练集上的行为，T(u)是用户在测试集上的行为

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/4.png)

> 用python代码求precision和recall：

```python
   def PrecisionRecall(test, N):
       hit = 0
       n_recall = 0
       n_precision = 0
       for user, items in test.items():
           rank = Recommend(user, N)
           hit += len(rank & items)
           n_recall += len(items)
           n_precision += N
       return [hit / (1.0 * n_recall), hit / (1.0 * n_precision)]
```

3. 覆盖率（描述一个推荐系统对物品长尾的发掘能力）

假设系统的用户集合为U，推荐系统给每个用户推荐一个长度为N的物品列表R(u):

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/5.png)

一个好的推荐系统不仅需要有比较高的用户满意度，也要有较高的覆盖率。为了更细致地描述推荐系统发掘长尾的能力，需要统计推荐列表中不同物品出现次数的分布。如果所有的物品都出现在推荐列表中，且出现的次数差不多，那么推荐系统发掘长尾的能力就很好。

> 在信息论和经济学中有两个著名的指标可以用来定义覆盖率

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/6.png)

这里p(i)是物品i的流行度除以所有物品流行度之和

这里$$i_{j}$$是按照物品流行度p()从小到大排序的物品列表中第j个物品

> 用python代码计算给定物品流行度分布后的基尼系数：

```python
   def GiniIndex(p):
       j = 1
       n = len(p)
       G = 0
       for item, weight in sorted(p.items(), key = itemgetter(1)):
           G += (2 * j - n - 1) * weight
       return G / float(n - 1)

```

> 基尼系数的计算原理

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/7.png)




#### 离线实验

1. 通过日志系统获得用户行为数据，并按照一定格式生成一个标准的数据集；

2. 将数据集按照一定的规则分成训练集和测试集；

3. 在训练集上训练用户兴趣模型，在测试集上进行预测；

4. 通过事先定义的离线指标评测算法在测试集上的预测结果。

> 离线实验优缺点

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/1.png)

#### 用户调查

因此，如果要准确评测一个算法，需要相对比较真实的环境。最好的方法就是将算法直接上线测试，但在对算法会不会降低用户满意度不太有把握的情况下，上线测试具有较高的风险，所以在上线测试前一般需要做一次称为用户调查的测试。

#### 在线实验

在完成离线实验和必要的用户调查后，可以将推荐系统上线做AB测试（AB测试是一种很常用的在线评测算法的实验方法。它通过一定的规则将用户随机分成几组，并对不同组的用户采用不同的算法，然后通过统计不同组用户的各种不同的评测指标比较不同算法，比如可以统计不同组用户的点击率，通过点击率比较不同算法的性能。）

AB测试的优缺点：

1. 可以公平获得不同算法实际在线时的性能指标;

2. 周期比较长，必须进行长期的实验才能得到可靠的结果,故只是用它测试那些在离线实验和用户调查中表现很好的算法；

3. AB测试系统的设计也是一项复杂的工程,切分流量是AB测试中的关键。

> AB测试系统

![image]({{ site.baseurl }}/assets/img/blog/2018-04-18-Recommender/2.png)

### 二、集成学习

GradientBoosting是一种常用的Boosting算法，试分析其与AdaBoost的异同

试分析随机森林为何比决策树Bagging集成的训练速度更快

试设计一种能提升k近邻分类器性能的集成学习算法

### 三、聚类

试分析AGENS算法使用最小距离和最大距离的区别

试设计一个能自动确定聚类数的改进k均值算法

### 四、降维和度量学习

在实践中，协方差矩阵$$XX^T$$的特征值分解常由中心化后的样本矩阵X的奇异值分解代替，试述其原因

试述核化线性降维与流行学习之间的联系及优缺点

### 四、特征选择与稀疏学习

试分析岭回归和支持向量机的联系

试述字典学习与压缩感知对稀疏性利用的异同

####























