---
layout: post
title: KDD2014 Practical Lessons from Predicting Clicks on Ads at Facebook笔记
categories: [machinelearning]
description: 2014GBDT+LR Facebook CTR预估论文翻译笔记
keywords: GBDT,LR,CTR,Facebook,predict,rank,笔记,翻译
---

# 论文
[Practical Lessons from Predicting Clicks on Ads at Facebook](https://research.fb.com/wp-content/uploads/2016/11/practical-lessons-from-predicting-clicks-on-ads-at-facebook.pdf)

# 为什么读这篇
本文属于深度学习时代之前的一篇经典CTR预估paper，即大名鼎鼎的GBDT+LR，在该模型之前大多是UserCF、ItemCF，wide & deep等深度学习出现后，逐渐退出舞台了，不过为了夯实下基础，还是得看看，同时希望可以从历史经典中挖出什么一些新的点子。

# 背景介绍
14年的KDD，出自脸书，总共11个作者，有华人有老外，有趣的是在本文发表时各个作者就已经各奔东西了，去哪的都有，Square，Quora，Twitter，Amazon。一作Xinran He现在在Snap，本篇是他读博二时在Facebook实习的工作。90后，08年P大本科，12年南加州PHD。与作者同样是90后差距太大了。

# 摘要
Facebook的日活7.5亿，广告主1百万（14年）。本文提出的模型结合了决策树和逻辑回归(GBDT+LR)预测广告CTR(点击率)，相比各自方法有3%的提升。也探索了一些基本参数是如何影响系统的最终预测性能。这会对整体系统的表现的提升产生重大影响。作者探索了一些基础参数怎么影响系统的最终预测表现。不出意外的是，最重要的事情是拥有正确的特征：捕获到的用户和广告的历史信息支配着其他类型的特征。一旦我们有了正确的特征和正确的模型（决策树加逻辑回归），其他的因素就影响很小（虽然小的改进在规模上很重要）。选择对数据新鲜度，学习率模式，和数据采样的最佳处理能够轻微的改变模型，但远不如一开始就选择高价值的特征或选择正确的模型。
       
简而言之，文章提出了一种**利用GBDT自动进行特征筛选和组合，进而生成新的feature vector，再把该feature vector当作logistic regression的模型输入，预测CTR的模型结构**。

# 1. 介绍
数字广告是一个数十亿美元规模的产业，并且逐年高速增长。大多数在线广告平台的广告投放是动态调整的，并基于观测用户反馈的兴趣进行定制的。机器学习在计算候选广告对用户的期望效用上起到主要作用，并以此提高市场效率。

Varian和Edelman等人于2007年发表的有影响力的论文描述了由Google和Yahoo开创的按点击次数出价和付费的拍卖模型。同一年，微软也基于同样的拍卖模型(auction model)搭建了赞助搜索市场。广告拍卖的效率依赖于点击预测的精度和校准(calibration)。点击预测系统必须具有稳健性和适应性，并且能够胜任从海量数据中学习。本文的目标是分享从实验及真实应用中获得的见解。

在赞助搜索广告中，用户查询(query)用于检索和查询匹配的候选广告。在Facebook，广告不是和查询关联，而是通过人口统计(specify demographic)和兴趣确定目标，因此用户访问Facebook时符合展示的广告量比赞助搜索更大。

用户访问Facebook会触发广告请求，而每次广告请求需要处理大量的候选广告，所以我们构建了计算成本逐级升高的**级联分类器**。本文重点介绍最后一个阶段的**点击预测模型**。

这篇论文的目的就是分享用真实世界的数据，并且具有鲁棒性和适应性的实验中得到的见解。

Facebook由于其特殊性，不能使用搜索记录进行广告推荐，而是基于对用户的定位，所以可展示广告的量也更多。Facebook为此建立的级联分类器。这篇文章专注于级联分类器的最后一个阶段点击预测模型，就是这个模型对最终的候选广告集的广告进行预测是否会被点击。

**级联分类器**

级联是基于几个分类器的串联的集合学习的特定情况，使用从给定分类器的输出收集的所有信息作为级联中的下一个分类器的附加信息。与投票或堆叠合奏（多专家系统）不同，级联是多阶段的。


# 2. 实验

## 2.1 构建training data和testing data
为了实现严格可控的实验，我们选择2013年第四季度任意一周的数据作为离线训练数据。为了保持不同条件下训练数据和测试数据的一致性，准备的离线训练数据要和在线观测的数据相似。我们将存储的离线数据分割成训练数据和测试数据，并使用它们模拟用于在线训练和预测的流数据。

## 2.2 评估指标Evaluation metrics：

由于我们最关注最重要的因素对机器学习模型的影响，因此我们使用预测的准确性，而不是直接与利润和收入相关的指标。在这项工作中，我们使用归一化熵（NE）和校准作为我们的主要评估指标。

### 2.2.1 Normalized Entropy（NE）：

Normalized Entropy归一化熵的定义为每次展现时预测得到的log loss的平均值，除以对整个数据集的平均log loss值。之所以需要除以整个数据集的平均log loss值，是因为**backgroud CTR**(背景点击率是训练数据集的平均经验点击率)越接近于0或1，则越容易预测取得较好的log loss值，而做了normalization后，NE便会对backgroud CTR不敏感了。

NE在计算相关的信息增益时是至关重要的。上面是逻辑回归的损失函数，也就是交叉熵。下面是个常数，所以这个Normalized Entropy值越低，则说明预测的效果越好。下面列出表达式：

![Normalized Entropy（NE）](/img/paper/lr_gbdt/ne.png)

其中N是训练数据集样本数，标签yi取值为-1或+1，pi是预估点击概率，p是平均经验CTR（即广告实际点击次数除以广告展示量）。

NE本质上是计算相对信息增益（RIG）的一个组成部分，RIG = 1 - NE

### 2.2.2 Calibration校正：

Calibration定义为平均预估CTR(the average estimated CTR)和经验CTR(empirical CTR)的比率，即期望的点击数和实际观测的点击数的比率。Calibration 是一个很重要的指标，因为CTR的精确性和校准对在线出价和拍卖的成功至关重要。 该值越接近于1，模型效果越好。

**AUC**

注意，ROC下面积（AUC）也是衡量排序质量（不考虑校准）的一个很好的指标。在现实的环境中，我们希望该预测是准确的，而不是仅仅获得最佳排名，以避免潜在的广告投放不足或广告投放过多。 NE可以衡量预测的良好性，并直接反映校准。例如，如果一个模型高估了2倍，并且我们应用了一个全局乘数0.5来修改校准，那么即使AUC保持不变，相应的NE也将得到改善。

上面那句话的意思是**NE衡量预测的良好性反应校准，AUC衡量排名质量而不考虑校准。**

# 3. 预测模型结构

本节评估不同的概率线性分类器和不同的在线学习算法。

本节提出一种混合模型结构：提升决策树和概率稀疏线性分类器的串联。，如下图所示。

3.1节说明了决策树是非常强大的输入特征转换器，能够显著提升概率线性模型的精确度。

3.2节说明了更新鲜的训练数据能够使预测更精确，从而激发了一个想法：使用在线学习方法训练学习器。

3.3节比较了两种线性分类器的几个变体。

![lr_gbdt_struct.jpg](/img/paper/lr_gbdt/lr_gbdt_struct.jpg)


学习算法是用的是Stochastic Gradient Descent(SGD)随机梯度下降，或者Bayesian online learning scheme for probit regression(BOPR)贝叶斯概率回归在线学习方案都可以。本文评估的在线学习方案基于应用于稀疏线性分类器的SGD算法，原因是资源消耗要小一些。在特征变换后，曝光的广告是以结构化向量的形式给出的：![3_0_1](/img/paper/lr_gbdt/3_0_1.svg)，其中![Alt text](/img/paper/lr_gbdt/3_0_2.svg)是第i个单元向量，而![Alt text](/img/paper/lr_gbdt/3_0_3.svg)是n个分类输入特征的值。在训练阶段，假设给定二元标签![Alt text](/img/paper/lr_gbdt/3_0_4.svg)来表示点击或非点击。

给定带标签的曝光广告![Alt text](/img/paper/lr_gbdt/3_0_5.svg)，定义有效权重的线性组合定义为公式2：

![Alt text](/img/paper/lr_gbdt/3_0_6.svg)

其中![Alt text](/img/paper/lr_gbdt/3_0_7.svg)为线性点击分的权重向量。

在SOTA的BOPR（Bayesian online learning scheme for probit regression）算法中，似然度和概率如下定义：

[Alt text](/img/paper/lr_gbdt/3_0_8.svg)

其中[Alt text](/img/paper/lr_gbdt/3_0_9.svg)为标准正态分布的累积密度函数，[Alt text](/img/paper/lr_gbdt/3_0_10.svg)为标准正态分布的密度函数。通过期望传播和矩匹配来实现在线训练。结果模型由权重向量的近似后验分布的均值和方差组成。

本文比较了BOPR与似然函数的SGD，[Alt text](/img/paper/lr_gbdt/3_0_11.svg)。产生的算法通常称之为逻辑回归（LR）。

SGD和BOPR都可以针对单个样本进行训练，所以他们可以做成流式的学习器(stream learner)。

## 3.1 决策树特征转换

## 3.2 数据新鲜度
## 3.3 在线线性分类器

# 4. 在线数据连接器
# 5. 内存和延迟的均衡
## 5.1 number of boosting tree
## 5.2 Boosting feature importance
## 5.3 Historical features

# 6. COPING WITH MASSIVE TRAINING DATA
## 6.1 Uniform subsampling
## 6.2 Negative down sampling
## 6.3 Model Re-Calibration

# 7. 结论

# 个人想法

# 参考
- [腾讯云社区 论文翻译 Practical Lessons from Predicting Clicks on Ads at Facebook](https://cloud.tencent.com/developer/article/1559589)
- [知乎 王喆：回顾Facebook经典CTR预估模型](https://zhuanlan.zhihu.com/p/32321996)
- [简书 论文笔记 KDD2014 Practical Lessons from Predicting Clicks on Ads at Facebook](https://www.jianshu.com/p/698d02b20916)
- [zdkswd.github.io Practical Lessons from Predicting Clicks on Ads at Facebook](https://zdkswd.github.io/2018/11/16/Practical%20Lessons%20from%20Predicting%20Clicks%20on%20Ads%20at%20Facebook/)