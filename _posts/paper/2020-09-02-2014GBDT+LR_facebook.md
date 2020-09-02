---
layout: post
title: KDD2014 Practical Lessons from Predicting Clicks on Ads at Facebook笔记
categories: [machinelearning]
description: 2014GBDT+LR Facebook CTR预估论文笔记
keywords: GBDT,LR,CTR,Facebook,predict,rank
---

## 论文
[Practical Lessons from Predicting Clicks on Ads at Facebook](https://research.fb.com/wp-content/uploads/2016/11/practical-lessons-from-predicting-clicks-on-ads-at-facebook.pdf)

## 为什么读这篇
本文属于深度学习时代之前的一篇经典CTR预估paper，即大名鼎鼎的GBDT+LR，在该模型之前大多是UserCF、ItemCF，wide & deep等深度学习出现后，逐渐退出舞台了，不过为了夯实下基础，还是得看看，同时希望可以从历史经典中挖出什么一些新的点子。

## 背景介绍
14年的KDD，出自脸书，总共11个作者，有华人有老外，有趣的是在本文发表时各个作者就已经各奔东西了，去哪的都有，Square，Quora，Twitter，Amazon。一作Xinran He现在在Snap，本篇是他读博二时在Facebook实习的工作。90后，08年P大本科，12年南加州PHD。与作者同样是90后差距太大了。

## 摘要
Facebook的日活7.5亿，广告主1百万（14年）。本文提出的模型结合了决策树和逻辑回归(GBDT+LR)预测广告CTR(点击率)，相比各自方法有3%的提升。也探索了一些基本参数是如何影响系统的最终预测性能。这会对整体系统的表现的提升产生重大影响。作者探索了一些基础参数怎么影响系统的最终预测表现。不出意外的是，最重要的事情是拥有正确的特征：捕获到的用户和广告的历史信息支配着其他类型的特征。一旦我们有了正确的特征和正确的模型（决策树加逻辑回归），其他的因素就影响很小（虽然小的改进在规模上很重要）。选择对数据新鲜度，学习率模式，和数据采样的最佳处理能够轻微的改变模型，但远不如一开始就选择高价值的特征或选择正确的模型。
       
简而言之，文章提出了一种**利用GBDT自动进行特征筛选和组合，进而生成新的feature vector，再把该feature vector当作logistic regression的模型输入，预测CTR的模型结构**。

## 介绍
数字广告是一个数十亿美元规模的产业，并且逐年高速增长。大多数在线广告平台的广告投放是动态调整的，并基于观测用户反馈的兴趣进行定制的。机器学习在计算候选广告对用户的期望效用上起到主要作用，并以此提高市场效率。

Varian和Edelman等人于2007年发表的有影响力的论文描述了由Google和Yahoo开创的按点击次数出价和付费的拍卖模型。同一年，微软也基于同样的拍卖模型(auction model)搭建了赞助搜索市场。广告拍卖的效率依赖于点击预测的精度和校准(calibration)。点击预测系统必须具有稳健性和适应性，并且能够胜任从海量数据中学习。本文的目标是分享从实验及真实应用中获得的见解。

在赞助搜索广告中，用户查询(query)用于检索和查询匹配的候选广告。在Facebook，广告不是和查询关联，而是通过人口统计和兴趣确定目标，因此用户访问Facebook时符合展示的广告量比赞助搜索更大。

用户访问Facebook会触发广告请求，而每次广告请求需要处理大量的候选广告，所以我们构建了计算成本逐级升高的级联分类器。本文重点介绍最后一个阶段的点击预测模型。

这篇论文的目的就是分享用真实世界的数据，并且具有鲁棒性和适应性的实验中得到的见解。

Facebook由于其特殊性，不能使用搜索记录进行广告推荐，而是基于对用户的定位，所以可展示广告的量也更多。Facebook为此建立的级联分类器。这篇文章专注于级联分类器的最后一个阶段点击预测模型，就是这个模型对最终的候选广告集的广告进行预测是否会被点击。

### 生成LateX公式
生成Latex公式可以使用word自带的公式编辑器、MathType、或者Latex公式在线编辑器等，这里以word为例。

1. Microsoft Word2016以上或者Office365.
2. 输入你想转换的公式.
3. 在Word 2016中选择该公式，上面的选项卡在“设计”一栏。
4. 在选项卡中选择 “{}LaTeX” 选项：

![Alt text](/img/md/formula1.png)

5. 复制这个公式：

![Alt text](/img/md/formula2.png)

### 2.3 拷贝公式到markdown

公式两边需要加$或者$$（不太明白为什么有些公式加一个$，有些却需要加两个$$才能显示），粘贴到markdown中，最后公式效果如下：

$${|\left|x\right||}_p={(\sum_{i}{|x_i|}_p)}^\frac{1}{p}$$

$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$

## 参考
[王喆：回顾Facebook经典CTR预估模型](https://zhuanlan.zhihu.com/p/32321996)
[论文翻译|Practical Lessons from Predicting Clicks on Ads at Facebook](https://cloud.tencent.com/developer/article/1559589)
[论文笔记 | KDD2014 | Practical Lessons from Predicting Clicks on Ads at Facebook](https://www.jianshu.com/p/698d02b20916)
[]()