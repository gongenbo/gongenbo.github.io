---
layout: post
title: 机器学习周志华思维导图笔记
categories: [machinelearning]
description: 周志华机器学习思维导图笔记
keywords: 周志华,机器学习,思维导图,笔记
---

# 机器学习——周志华

## 简介

这篇文章是使用xmind导出的，所以图片有丢失，完整版《机器学习》周志华、思维导图脑图下载地址如下：
[https://github.com/gongenbo/gongenbo.github.io/tree/master/img/ml/zhouzhihua](https://github.com/gongenbo/gongenbo.github.io/tree/master/img/ml/zhouzhihua)

下面附图片版
![Alt text]({{site.url}}/img/ml/zhouzhihua/ml_zhouzhihua.png)
## 绪论

### 基本术语

- model, learning algorithm, 
- 数据结构
data set, insatance, sample, attribute, feature, 
attribute space, sample space, feature vector
- 模型训练概念
learning, training, training data, training sample,
training set, hypothesis, ground-truth,  learner
(xi, yi)
- 方法的分类
classification, regression, binary classification, 
positive class, negative class, multi-class classification,
- 评价模型，聚类，分布概念
testing, testing sample, clustering, cluster, 
generalization, distribution, independent and indentically

### 假设空间

- 归纳，演绎
induction, deduction, generalization, specialization, inductive learning, concept
- 所有假设空间

### 归纳偏好

- 算法的归纳偏好
- Occam's razro 奥卡姆剃刀
- no free lunch

## 模型评估与选择

### 检验误差与过拟合

- error rate错误率

	- E=a/m=错误样本数/总个数

- accuracy精度

	- 1-error rate

- 概念

	- error, training error, 
empirical error, generalization error, 
overfitting, underfitting

### 评估方法

- 测试集

	- testing set
testing error

- 数据划分方法

	- 留出法hold out

		- 将集合分为互斥的两个部分,一部分作为训练集，一部分作为测试集
		- 尽可能两个集合分布的一致

			- 分层采样stratified sample

		- 进行多次划分及测试，将多次结果取错误率的平均值

	- 交叉验证法cross validation

		- k个互斥的子集合
		- 划分使用分层采样
		- 极端k=m，leave one out留一法

	- 自助法bootstrapping

		- 有放回抽样，抽取m个
		- 使用包外估计(out of bag),即没有被抽样的部分作为测试集合
		- 小数据适合，大数据使用交叉验证或者留出法比较好

- 调参和最终模型

	- 概念

		- parameter, parameter tuning

	- 集合划分(训练集+验证集(调参)+测试集)

	  验证集用于调参

### 比较检验（检验算法好坏）

- 单样本
- 配对样本

	- 交叉检验的配对检验
	- McNemar检验

- 多样本

	- Friedman检验

		- 测试性能排序
		- 计算统计量
		- 查临界值表

	- Nemenyi检验

### 性能度量performance measure

- 参考链接

- 基本指标

	- 【概念】

	   

		- true positive, false positive,
true negative, false negative,
confusion matrix

	- 【均方误差】mean squared error

		- Subtopic
		- Subtopic

	- 【错误率】error rate

		- Subtopic
		- Subtopic

	- 【精度, 准确率】accuracy

		- 正确分类的样本/总样本：(TP+TN)/(ALL)
		- 在不平衡分类问题中难以准确度量：
比如98%的正样本只需全部预测为正即可获得98%准确率

	- 【查准率】precision

		- TP/(TP+FP)：在你预测为1的样本中实际为1的概率
		- 查准率在检索系统中：检出的相关文献与检出的全部文献的百分比，衡量检索的信噪比

	- 【召回率】【查全率】recall

		- TP/(TP+FN)：在实际为1的样本中你预测为1的概率

	- 【真正例率】和【假正例率】

		- Subtopic
		- ROC受试者工作特征（Recever Operating Characteristic）
		- AUC(Area Under ROC Curve)

- 三种衡量图

	- P-R图

		- 图形说明与解析

			- 横轴：查全率recal，R
			- 纵轴：查准率precision，P
			- 图形
			- 图形解析

		- 指标

			- Break-Event Point BEP平衡点
			- F1度量

				- Subtopic

			- Fβ度量

				- Subtopic
				- 说明

			- F1,Fβ度量来源

				- Subtopic
				- β平方

		- 多分类

			- 宏查准率macro-P,宏查全率macro-R,宏F1(macro-F1)
			- 微查准率micro-P,微查全率micro-R,微F1(micro-F1)

	- ROC,AUC图

		- 背景

			- 使用分类阈值threshold，
截断点cut point

			   

			- 来源简介

			   

		- 图形说明与解析

			- 横轴：假阳性率
			- 纵轴：真阳性率
			- 图形ROC, AUC，解析
			- 图形绘制方法

			  1，将分类阈值设置为最大2，将分类阈值逐渐变小，通过变化分类阈值得到ROC曲线

			- AUC计算法，loss

	- 代价曲线

		- 背景

			- 非均等代价unequal cost

		- 图形说明及解析

			- 横轴：正例概率代价
			- 纵轴：归一化代价
			- 图形及绘制

### 偏差与方差bias-variance decomposition

- 分解公式

   

- 偏差，方差，

   

- 图形分析

## 线性模型

### 线性回归

- 基本

	- 优点：可解释性comprehensibility
	- 数据类型
	- 模型

- 参数推导

	- 单元
	- 多元

		- 补充

	- 矩阵求导

	  https://en.wikipedia.org/wiki/Matrix_calculus

- 广义线性回归(generalized linear model)

	- Subtopic

### 对数几率回归

- 对数几率函数(logistic function)由来

	- 基本思想：用sigmod函数拟合概率
	- y视作取正例的概率

		- Subtopic

	- 1-y视为取负例的概率

		- Subtopic

	- y/(1-y)几率
	- ln(y/(1-y))=wx+b对数几率

- 原理及求解方法

	- 最大化

		- Subtopic

	- 最小化

		- Subtopic

- 参数推导

### 线性判别分析

- 线形判别分析
(Linear Discriminant Analysis 简称LDA)

  LDA要求各类样本的协方差矩阵相同且满秩

- Fisher判别分析

  线性判别分析最早由Fisher提出，解决二分类

- 原理
- 常见的监督降维技术
- PDA,QDA是无监督的降维

	- 都需要均值不同，方差相同

### 多分类学习

- 经典拆分序列

	- 一对一OvsO
	- 一对其余OvsR(one vs rest)
	- 多对多MvsM(many vs many)

		- 原理

	- 示意图

### 类别不平衡问题

- 上述学习方法需要类别大致相当
- 简介
- 再平衡方法（前提:无偏采样）
- 如果不是无偏采样，处理为接近真实数据(过采样,欠采样,阈值移动)

## 决策树

### 划分选择

- 信息熵 information entropy

	- Subtopic

- 信息熵增益(ID3算法)

  划分前的信息熵-划分后各个子类的熵的加权和（权重为子类个数）

	- Subtopic
	- ID3(Iterative Dichotomise迭代二分器)
	- 缺点:偏好类别多的个体
	- 例子

- 增益率(C4.5算法)

  偏好类别少的个体，所以采用启发式，详见步骤

	- Subtopic
	- 步骤

		- 1.选出信息增益大于平均值的属性
		- 2.选择增益率最大的

- 基尼指数(CART)

	- 指标意义:随机选择两个元素类别不一致的可能

		- Subtopic

	- 属性的基尼指数

		- Subtopic

	- CART算法

		- Subtopic

	- 选取准则

	  基尼系数越小，纯度越高，作为最优划分属性
	  优点：减少了信息熵log计算量

### 剪枝处理pruning

- 预剪枝prepruning

	- 生成过程中剪枝
	- 图解
	- 缺点:可能造成欠拟合

	  预剪枝使得决策树的很多分支都没有“展开”，这不仅降低了过拟合的风险，还显著减少了决策树的训练时间开销和测试时间开销，但另一方面，有些分支的当前划分虽不能提升泛化性能、甚至可能导致泛化性能暂时下降，但在其基础上进行的后续划分却有可能导致性能显著提高；预剪枝基于“贪心”本质禁止这些分支展开，给预剪枝决策树带来了欠拟合的风险。

- 后剪枝post-pruning

	- 生成后剪枝
	- 图解
	- 缺点:可能过拟合

	  后剪枝决策树通常比预剪枝决策树保留了更多的分支。一般情形下，后剪枝决策树的欠拟合风险很小，泛化性能往往优于预剪枝决策树。但后剪枝过程是在生成完全决策树之后进行的，并且要自底向上地对数中的所有非叶结点进行逐一考察，因此其训练时间开销比未剪枝决策树和预剪枝决策树要大得多。

### 连续和缺失值

- 连续值处理

	- 二分法bi-partition(C4.5中的方法)
	- 方法详细说明
	- 划分法

		- Subtopic

	- 增益熵计算

		- Subtopic

- 缺失值的处理

### 多变量决策树
multivariate tree

- 单变量(划分属性时单变量)决策树回顾
- 简介:非叶子节点都是属性的线性组合,
划分的时候考虑多个属性
- 示例
- OC1算法

## 神经网络

### 神经元模型

- M-P神经元模型
- 常见激活函数
activation function

   

	- 一些术语

		- 饱和

			- 软饱和:能够对噪音更加鲁棒,但是梯度容易消失

		- 梯度

			- 梯度消失

			  （1）梯度消失：根据链式法则，如果每一层神经元对上一层的输出的偏导乘上权重结果都小于1的话，那么即使这个结果是0.99，在经过足够多层传播之后，误差对输入层的偏导会趋于0。可以采用ReLU激活函数有效的解决梯度消失的情况。

			- 梯度膨胀

			  梯度膨胀：根据链式法则，如果每一层神经元对上一层的输出的偏导乘上权重结果都大于1的话，在经过足够多层传播之后，误差对输入层的偏导会趋于无穷大。可以通过激活函数来解决

				- 处理方式

					- 阈值剪切clip

					  梯度剪切、正则梯度剪切这个方案主要是针对梯度爆炸提出的，其思想是设置一个梯度剪切阈值，然后更新梯度的时候，如果梯度超过这个阈值，那么就将其强制限制在这个范围之内，通过这种直接的方法就可以防止梯度爆炸。

					- relu激活函数

			- batchnorm
（batch normalization）

				- 每层输出进行标准化

		- 网络的退化问题

			- 网络层数增加，但是在训练集上的准确率却饱和甚至下降了。

		- 残差网络
ResNet

			- Deep Residual Learning for Image Recognition
			- 图解

		- LSTM
		- 神经元死亡

	- sigmod

		- Subtopic
		- 缺点:梯度消失,软饱和;偏置
		- Subtopic

	- tanh

		- Subtopic
		- 缺点:软饱和
		- 优点:不偏置
输出均值为0，这就使得其收敛速度要比sigmoid快，从而可以减少迭代次数。

			- Subtopic

		- Subtopic

	- ReLu
(Rectified Linear Units)

		- Subtopic
		- 缺点:偏移现象和神经元死亡
		- Subtopic

	- ReLu的修正

		- Leaky-ReLU

			- Subtopic
			- 避免ReLU在x<0时的神经元死亡现象

		- ELU

			- Subtopic
			- 左侧软饱和:能够对噪音更加鲁棒

		- Subtopic

### 感知机和多层网络
perceptron

- 感知机

	- 两层神经元组成
	- 组成

	  在·

	- 学习率
	- 图解

- 多层网络

   

	- 前馈含义:拓扑结构不存在环或回路
	- 多层网络:包含隐含层
	- 连接权connection weight

### 误差逆转传播算法
error BackPropagation(BP)

- 图解
- 优化方法:通过链式求导法则
- 缓解过拟合方法

	- 早停:dropout
	- 正则化:regularization

		- Subtopic

### 全局最小和局部极小

- 定义
- 图解
- 避免局部最小的方法

	- 从不同初始值多训练几次
	- 模拟退火
	- 换梯度下降方式

### 其他神经网络

- RBF网络

	- 特点:激活函数使用径向基函数(Radial Basis Function)

		- Subtopic
		- Subtopic
		- Subtopic

	- 步骤

		- 1寻找中心c
		- 2训练参数

- ART网络

	- 特点：每一时刻只有一个神经元被激活

- SOM网络

	- 特点:将低维映射到高维

- 级联相关网络

	- 特点:训练网络结构

- RNN

	- 特点：网络出现环

- Boltzmann机

	- 特点:为网络定义"能量函数"

### 扩展，梯度下降的方法

- 梯度下降方法

	- 参考链接

	  https://www.cnblogs.com/guoyaohua/p/8542554.html http://ruder.io/optimizing-gradient-descent/index.htmlhttps://blog.csdn.net/qq_21997625/article/details/80271300

	- 三种下降策略

		- 批量梯度下降BGD(Batch gradient descent)

			- Subtopic
			- BGD 采用整个训练集的数据来计算 cost function 对参数的梯度
			- 优点（凸函数）能够下降到局部最优或者全局最优，迭代次数较少
			- 缺点 应用所有数据集，速度慢，新数据不能及时更新
			- Subtopic

		- 随机梯度下降SGD(Stochastic gradient descent)

			- Subtopic
			- SGD对每个新的数据进行cost function的更新
			- 优点:能够实时更新，学习率缓慢下降，能够和SGD、BGD效果一样
			- 缺点:由于单个样本噪声大，收敛过程波动大
			- Subtopic
			- Subtopic

		- 小批量梯度下降MBGD(Mini-batch gradient descent)
最常见

			- Subtopic
			- SGD每次对一部分数据进行下降，n一般50-256
			- 优点:降低更新方差；充分地利用深度学习库中高度优化的矩阵操作进行更有效的梯度计算
			- 缺点:可能陷入局部最优
			- Subtopic

	- 主要挑战

		- 学习率的大小设定困难

			- 太大:在最小值附近震荡而不收敛
			- 太小:收敛速度太慢
			- 处理方式

				- 模拟退火算法

				  https://blog.csdn.net/weixin_42398658/article/details/84031235 

					- 以一定概率向损失增高的方向变
					- 原理
					- 步骤

				- 预先设定阈值

					- 如果两次下降的差异小于设定值，则降低学习率
					- 无法自适应，也无法反应数据

		- 不同数据应用学习率导致的问题

			- 所有数据都是用一个学习率，导致出现频率不同的数据使用一个学习率
			- 希望修正:频率低的数据给予更高的权重

		- 可能会收敛到局部最优点

	- 具体下降算法

	  https://alphafan.github.io/posts/grad_desc.html

		- 梯度下降不稳定

			- Momentum

				- Subtopic

					- 比较原始MGD
					- 一般 γ 取值 0.9 左右。

				- 对梯度进行修正，在原有的方向上进行梯队修正
				- 优点:梯度方向不变的维度上速度变快，梯度方向有所改变的维度上的更新速度变慢，这样就可以加快收敛并减小震荡。
				- 缺点:一定会往梯度下降的方向走，不能预先判断损失变化

			- (NAG)Nesterov Accelerated Gradient

				- Subtopic
				- 优点:在更新梯度时顺应 loss function 的梯度来调整速度，并且对 SGD 进行加速

		- 学习率自适应

			- Adagrad（Adaptive gradient algorithm）

				- Subtopic

					- 公式

					  记t时刻，参数i的梯度为:普通SGD更新策略为：adagrad更新策略为：Gt,ii为第i个参数的梯度平方和，为滑动平均值 

				- 降学习率设定为梯度和t相关的函数，f(t,θ梯度)
				- 优点:学习率能够适应梯度的变化
				- 缺点:分母不断累积，学习率会变得很小

			- Adadelta

				- Subtopic

					- Subtopic

						- Subtopic
						- Subtopic
						- Subtopic

					- Subtopic
					- γ 一般设定为 0.9

				- 使用了梯度的衰减平均值
				- 优点:能够避免分母累积，解决 Adagrad 学习率急剧下降问题的

			- RMSprop

				- Subtopic
				- Adadelta的特例

		- 学习率和梯度方向都修改

			- Adaptive Moment Estimation (Adam)

				- Subtopic

					- Subtopic
					- Subtopic
					- 梯度的指数衰减平均值mt(类似momentum)
					- 梯度的平方 vt(类似RMSprop )
					- 超参数设定值:

						- 建议 β1 ＝ 0.9，β2 ＝ 0.999，ϵ ＝ 10e−8

				- 相当于 RMSprop + Momentum
				- 优点:自适应学习率同时保证波动幅度不会太大

			- Nadam

				- Subtopic
				- Nesterov动量项的Adam

			- AMSGrad

				- Subtopic

					- Subtopic

				- 使用梯度的最大值而非指数平均
				- 避免陷入局部最优

	- 多线程计算

		- http://ruder.io/optimizing-gradient-descent/index.html

## 支持向量机

### 间隔和支持向量

- 原理:寻找一个分割平面能够将两类元素分开
- 图解
- 优化目标

	- Subtopic
	- Subtopic
	- 支持向量机基本型
	- 推导过程

	   

	- 图解

### 对偶问题

- 求解和推导过程(略)
- KTT条件
- 参考链接

  https://baijiahao.baidu.com/s?id=1618344063706450694&wfr=spider&for=pchttps://www.cnblogs.com/liaohuiqiang/p/7805954.htmlhttps://blog.csdn.net/qq_38517310/article/details/79494934

### 核函数

- 能将x，y映射为高维空间下的内积

	- 由来
	- 直接映射到高维空间计算困难
	- Subtopic

- 求解结果

	- Subtopic

- 构成核函数条件
- 核函数构成

	- 常用核函数

	   

	- 核函数的组合

		- Subtopic
		- Subtopic
		- Subtopic

### 软间隔(soft margin)和正则化

- 原理：可以有部分样本不符合要求
- 图解
- 优化目标

	- Subtopic
	- Subtopic

		- 松弛变量

	- 替代损失L

- 结构风险and经验风险

### 支持向量回归

- 原理
- 优化目标

	- Subtopic
	- Subtopic

- 解

	- Subtopic

- 加入核函数的解

	- Subtopic

### 核方法

- 表示定理
- 核化
- 通过核化能将线性学习器转化为非线性学习器

## 贝叶斯分类

### 贝叶斯决策论

- 原理

	- 期望损失

		- Subtopic

			- Subtopic
			- 将实际为j识别为i的损失

	- 贝叶斯判定准则 (Bayes decision rule)

		- Subtopic
		- 贝叶斯最优分类器

- 最小化错误率的情况

	- 损失函数

		- Subtopic

	- 条件风险

		- Subtopic

	- 贝叶斯最优分类器

		- Subtopic
		- 对每个样本，最大化后验概率

	- 两种策略

		- 直接建模P(c|x)，判别式模型(discriminative models)
		- 由概率分布生成模型，生成模型(generative models)

- 生成式模型

	- 最大化后验概率

		- Subtopic
		- Subtopic

	- 名词

		- 先验概率prior

			- P(C)

		- 似然likelihood、条件概率class-conditional probability

			- P（x|C）

		- 证据因子evidence

			- P（x）

	- 最大化部分

		- 最大化后验概率P(c|x)等同于最大化P(c)P(x|c)

	- 缺点

		- 很多样本可能没有出现过

### 极大似然估计

- 两个学派

  https://www.sohu.com/a/215176689_610300

	- 频率学派

		- 分布的参数固定值

	- 贝叶斯学派

		- 分布的参数为一个分布（新分布的参数为超参）

- 似然likelihood

	- Subtopic

- 对数似然log-likelihood

	- Subtopic

- 例子

### 朴素贝叶斯分类器

- 属性条件独立假设(attribute conditional independence assumption)

	- Subtopic

- 最大化目标

	- Subtopic
	- Subtopic

- 对应参数计算方法

	- 离散

		- Subtopic
		- Subtopic

	- 连续

		- Subtopic
		- Subtopic

- laplace平滑化方法

	- Subtopic

- 例子
- 实际应用注意

	- 更新频率不频繁

		- 先保存需要的概率计算结果

	- 更新频率频繁

		- 懒惰学习方法，计算时才使用

	- 增量学习，降新增的数据更新概率

### 半朴素贝叶斯分类器(semi-naive Bayes classifiers)

- 独依赖估计(One-Dependent Estimator, ODE)

	- 每个属性在类别之外最多仅依赖于一个其他属性

- 模型

	- Subtopic

		- Pai xi所依赖的父属性

- SPODE(Super-Parent ODE)

	- 超父super-parent：所有属性都依赖与一个父属性
	- Subtopic

- TAN(Tree Augmented naive Bayes)

	- 步骤
	- 条件互信息

		- Subtopic

	- TAN实际只保留强相关属性

- AODE(Averaged One-Dependent Estimator)

	- 将SPODE聚合（平均化）

		- Subtopic

	- 平滑化

		- Subtopic

	- 优点:AODE无需模型选择，相比与PODE

### 贝叶斯网

### EM算法

- 用于估计隐变量
- 隐变量 latent variable

	- 未观测的变量

- 包括隐变量的最大化似然

	- Subtopic
	- X以观测变量
	- Z隐变量

- 边际似然 marginal likelihood

	- Subtopic

- 步骤

	- E步骤

		- Subtopic
		- 求隐变量的期望

	- M步骤

		- Subtopic
		- 由已知的变量，极大似然法，确定参数

## 集成学习

### 个体与集成

- 概念

	- ensemble learning集成学习：通过多个学习器完成任务

	  由多个不同的个体学习器组成，个体学习器至少不差于弱学习器。弱学习器泛指性能略优于随机猜测的学习器，二分类精度略高于50%

	- individual learner个体学习器
componnet learner(组件学习器)

		- 构成:C4.5 BP神经网络
		- 同质homogeneous

			- 由同一类学习器组成

		- 异质heterogenous

			- 由不同类学习器组成

	- 示意图

- 好而不同
- 两类集成学习方法

	- 串行

		- Boosting

	- 并行

		- Bagging
		- Random Forest

### Boosting

- 原理

	- 建立基学习器
	- 对训练样本分布进行调整
	- 关注更多分类错误的部分

- AdaBoost

	- Subtopic
	- 加性模型additive model

		- Subtopic

	- 指数损失函数exponential loss function

		- Subtopic

	- AdaBoost算法描述

		- 初始分布D0
		- 根据损失函数更新分布Dt

	- 用错分数据点来识别问题，通过调整错分数据点的权重来改进模型

- GBDT

  梯度提升树，Gradient Boosting Decision Tree

	- 通过负梯度来识别问题，通过计算负梯度来改进模型。
	- 以CART作为基分类器

- XGBoost

	- 支持线性分类器，相当于带L1和L2正则化项的逻辑斯蒂回归（分类问题）或者线性回归（回归问题）
	- 对代价函数进行了二阶泰勒展开，同时用到了一阶和二阶导数

### Bagging与随机森林

bootstrap Aggreating缩写

- 由来

	- 学习器间的独立:通过采样保证训练数据集不同,
保证学习器之间的独立性
	- 每个学习器只能使用小部分训练数据

- Bagging

	- 自主采样法boostrap sampling

		- 均匀分布地从样本中采样
		- 63.2%中会出现在训练集中

	- 学习器结合方法

		- 分类任务-简单投票法(可以结合置信度)
		- 回归任务-简单平均法

	- 算法描述
	- 复杂度分析
	- 包外估计

		- 样本外数据进行测试
		- 对样本的包外预测

			- Subtopic

		- 泛化误差

			- Subtopic

		- 包外样本用法

			- 决策树可以用来剪枝
			- 神经网络可以用来早停

	- 从方差和偏差的角度考虑

		- bagging主要时降低方差
		- 学习器受样本扰动

- 随机森林 Random Forest简称RF

	- 对数据集通过bagging选择部分数据
	- 对决策树节点

		- 从属性集合中选择k个
		- 推荐值log2d
		- 理由：RF的方差来自样本扰动，也来自属性的扰动

	- 效果比较

### 结合策略

- 结合策略的好处

	- 避免学习空间过大，学习器h(x)学习不了f(x)
	- 避免落入局部最优
	- 结合多个学习器，扩大将设空间（真实模式f(x)的分布空间）
	- 图解

- 几种策略

	- 平均法

		- 简单平均

			- Subtopic
			- 性能相近时使用

		- 加权平均

			- Subtopic
			- 性能差异大的时候

	- 投票法

		- 绝对多数投票法(majority voting)

			- Subtopic
			- 票数过半才进行预测，否则不进行预测

		- 相对多数投票法(plurality voting)

			- Subtopic

		- 加权投票法(weighted voting)

			- Subtopic

				- Subtopic

		- 硬投票与软投票
		- 类概率使用

	- 学习法

		- 使用多个学习器，将x转变为包含上一级预测结果的多元变量
		- Stacking

			- 初级学习器，次级学习器
			- 将初级学习器的预测结果结合标志y，作为新的标签

				- Subtopic

			- stacking算法
			- 训练集划分法

				- 使用初级学习器未使用的数据用于次级训练器的学习

			- 几种例子

### 多样性

- 误差-分歧分解(error-ambiguity decomposition)
- 多样性度量diversity measure

	- 预测结果列联表（contingency table）

		- Subtopic

	- 不合度量(disagreement measure)

		- Subtopic

			- Subtopic

	- 相关系数(correlation coefficient)

		- Subtopic

			- Subtopic

	- Q-statistic

		- Subtopic

			- Subtopic

	- k-statistic

		- Subtopic

			- Subtopic
			- p1两个分类器取得一致的概率
			- p2两个分类器偶然达成一致的概率
			- k取值意义，通常[0,1]可以为负数，越小越不一致

		- k-误差图

- 多样性增强

	- 输入

		- 数据样本扰动
		- 输入属性扰动

			- 对属性集合进行随机抽样（random subspace）
			- 属性减少：有利于提高学习速度，增强多样性
			- 属性太少的时候不合适
			- 算法描述

	- 模型

		- 随机设置不同参数
		- 参数的初始值不同
		- 负相关法(Negative Correlation)

	- 输出

		- 翻转法
		- 输出调制法Output Smearing
		- ECOC

## 聚类

### 聚类任务

- 名词介绍

	- 无监督学习unsupervised learning

		- 没有样本标记

	- 聚类(clustering)

- 聚类算法输出

	- 寻找一个全集的划分
	- 划分全集

		- Subtopic
		- Subtopic
		- Subtopic

	- 簇标记cluster label

		- Subtopic

	- 簇标记向量

		- Subtopic

	- 作用

		- 寻找数据的内在分布结构
		- 其他学习任务的前驱任务
		- 例子

### 性能度量

- 相关概念

	- 有效性指标 validity index
	- 样本簇

		- 聚类就是将样本集划分为互不相交的子集，即样本簇。

	- 聚类目标：簇内相似度高(intra-cluster)，簇间相似度低(inter-cluster)

- 两类性能度量指标

	- 外部指标（external index）

		- 与某个参考模型进行比较
		- 数据预处理：划分数据对

			- 将数据集分对
			- 根据两个模型中，数据对的分类结果划分数据集

		- 几种指标（越大越好）[0,1]

			- Jaccard系数

				- Subtopic

			- FM指数

				- Subtopic

			- Rand指数

				- Subtopic

	- 内部指标（internal index)

		- 直接考察聚类结果而不利用任何参考模型
		- 类的几个距离

			- 组内平均距离

				- Subtopic

			- 组内最大距离

				- Subtopic

			- 组间最小距离

				- Subtopic

			- 组间均值差

				- Subtopic

			- Subtopic
			- Subtopic

		- DBI(Davies-Bouldin Index)

			- Subtopic
			- 分母为C，综合考虑了对于每一类，与它最远的类，在进行平均

		- Dunn(Dunn Index DI)

			- Subtopic
			- 原理:考虑最接近的两类

### 距离计算

- 距离度量的性质

	- Subtopic

- Minkowski distance明可夫斯基距离
连续属性距离

	- p = n

		- Subtopic

	- p=1 manhattan distance曼哈顿距离

		- Subtopic

	- p=2 Euclidean distance欧式距离

		- Subtopic

	- Xi为一个样本，包括多个属性

- 离散属性的距离

	- VDM value difference metric

		- Subtopic

		  Value Difference Metric

		- Subtopic

			- 属性u取值为a

		- Subtopic

			- 第i类中，属性u取值为a的部分

		- 在各类中分别计算属性a,b取值关于总样本的比例，两个比例做差
		- 给予已有类别

- 混合属性距离

	- Minkowski distance and VDM

		- Subtopic

	- 加权距离

		- Subtopic
		- Subtopic

### 原型聚类

- prototype-based clustering名称意义

	- 原型指的是样本空间中具有代表意义的点

- k均值类算法

	- k均值算法(k-means)

		- 原型

			- 原型为k个均值（从随机选择k个样本开始）

		- 算法描述

			- 步骤

				- 随机选择k个
				- 对于别的样本Xi，计算其与哪类最为接近，进行归类
				- 重新计算各类的均值
				- 重复步骤2，3直到收敛

		- 目标:最小化平方误差

			- Subtopic
			- Subtopic
			- Subtopic
			- Subtopic

		- 分类示意图

	- k均值中心Partitioning Around Medoids

		- 原型

			- 原型为k个中心（从随机选择k个样本开始）

		- 算法描述

			- 选择k个样本作为中心
			- 对于别的样本Xi，计算其与哪类最为接近，进行归类
			- 重新选择各类中的中心
			- 重复步骤2,3

		- 参考

		  https://blog.csdn.net/u012711335/article/details/78699896https://www.cnblogs.com/king-lps/p/7775108.html

- 学习向量量化(LVQ Learning Vector Quantization)

	- 原型

		- 从k个向量开始，取k个向量为原型

	- 算法描述

		- 步骤

			- 使用空间中的k个向量作为原型
			- 计算新样本xi最接近的向量
			- 根据样本x和最接近向量p调整向量新方向

	- 原理

		- 假设数据样本都带有类别标记，学习过程利用样本的监督信息来辅助聚类

	- 划分方式（Voronoi剖分）
	- 例子
	- 分类示意图

- 高斯混合聚类(Mixture-of-Gaussian)

	- 原型

		- 假定样本由k个正态分布构成的，由k个分布构成原型

	- 假定x服从正态分布

		- 概率密度函数
		- Subtopic
		- mixture coefficient 混合系数
		- 高斯混合分布

	- 算法描述
	- 例子
	- 图解

### 密度聚类

- 假定聚类结构能通过样本分布的紧密程度确定
- DBSCAN算法相关概念

	- 图解

- 概念

	- 核心对象

	  若xj的邻域至少包含MinPts个样本

	- 密度直达

	  Xi是核心对象，则称Xj与Xi密度直达

	- 密度可达

	  p1=xi,pn=xj 且P(i+1)由Pi密度直达，则称Xj由Xi密度可达

	- 密度相连

	  第x+1个不是核心元素

- 簇的定义

	- 由密度可达关系导出的最大的密度相连样本集合

- 算法描述

	- 根据给定邻域参数找出所有核心对象
	- 以任一核心对象出发，找出其密度可达的样本生成聚类簇
	- 直到所有核心对象均被访问过为止

- 图解

### 层次聚类

- 层次聚类hierarchical clustering

	- 将每个样本认为为一簇

- 簇的距离定义

	- Subtopic
	- 集合间距离常用豪斯多夫距离

- AGNES算法描述

	- 自底向上的合并类

- 图解

### 扩展阅读

- 评价指标

	- 轮廓系数

		- 参考

		  https://blog.csdn.net/wangxiaopeng0329/article/details/53542606

		- Subtopic
		- Subtopic

			- 表示样本i与类内各样本距离的平均值

		- Subtopic

			- 表示样本i与不同类内各样本距离的平均值

		- 使用

			- 判断：

				- si接近1，则说明样本i聚类合理；
				- si接近-1，则说明样本i更应该分类到另外的簇；
				- 若si 近似为0，则说明样本i在两个簇的边界上。

	- AIC，BIC

	  https://blog.csdn.net/weixin_41500849/article/details/80321219

	- 参考

	  https://blog.csdn.net/weixin_33755847/article/details/86011805https://www.cnblogs.com/think90/p/7133753.html

## 降维与度量学习

### k-Nearest Neighbor
k近邻

- 寻找最接近的k个训练样本
- 预测方法

	- 分类使用投票法
	- 回归使用平均法

- lazy learning 懒惰学习

	- 收到新样本后才进行学习

- 图解
- kNN效能分析（不超过最优贝叶斯分类器的两倍）

### 低维嵌入

- 原理：降维后保持距离矩阵D和内积矩阵B不变

	- 距离矩阵D：第i行j列表示xi和xj之间的距离
	- 内积矩阵B：第i行j列表示xi和xj之间的内积

- 算法“多维缩放”
Multiple Dimensional Scaling MDS

	- 特征值分解
	- 步骤

		- 分解距离矩阵
		- 选取其中的一部分非零特征值构成新特征

			- Subtopic

	- 算法描述

### 主成分分析PCA
Principal Component Analysis

- 要求：正态分布，均值不同，方差相同
- 线性降维方法

	- Subtopic
	- W为d*d维数，X为d*m维数，d<<m

- 正交属性空间中，使用超平面（直线的高维推广）对所有样本进行恰当表达
- 原理

	- 将变量投影到合适的超平面，线性降维
	- 最近重构性：样本点到这个超平面的距离都足够近；

		- 重构后的向量与原向量距离最近

	- 最大可分性：样本点在这个超平面上的投影能尽可能分开；

- 两种推导

	- 最近重构性

		- Subtopic
		- 最小样本到超平面的距离

	- 最大可分性

	  样本点到超平面上的投影尽可能分开

		- 最大化方差

			- Subtopic

		- Subtopic
		- 图解

	- 拉格朗日法求解

	   

- 算法步骤

	- Subtopic
	- 1.计算XXt

		- Subtopic

	- 2.提取前d个特征值（最大的前d个）构成W

		- Subtopic
		- 通过开销较小的学习器，进行交叉验证，判断d取多少

	- 3.投影矩阵就是W

- 算法描述
- 比较和拓展

	- 与LDA比较

	  https://blog.csdn.net/Chenzhi_2016/article/details/79451201

	- QDA：针对均值不同，方差不同的多类别

	  http://www.mamicode.com/info-detail-1819236.html

	- SVD也可以用于PCA降维

### 核化线性降维
Kernelized PCA

- 对于需要非线性映射进行降维的情况
- 步骤

	- 1.求解A

		- Subtopic
		- Subtopic

	- 2.得到投影

		- Subtopic

- 推导过程

### 流形学习
manifold learning

低维嵌入到高维，数据样本在高维的局部仍具有欧式空间的性质，可以局部建立降维映射关系

- 等度量映射Isometric Mapping

	- 原理：高维中局部结构的距离与平面接近,试图保持临近样本间距离

		- 图解

	- 步骤

		- 1.先计算点之间，两两之间的距离
(Dijkstra算法，Floyd算法)
		- 2.计算的距离作为距离矩阵B输入MDS算法中

	- Isomap算法描述

		- 确定xi的k近邻
		- xi与k近邻点距离设为欧氏距离，与其他点设为无穷大
		- 调用最短路径（Dijkstra/Floyd）计算任意两样本之间的距离
		- 将dist(xi,xj)作为MDS算法的输入
		- 返回MDS算法的输出

- 局部线性嵌入
Locally Linear Embedding
LLE

	- 原理：保持领域内样本间的线性关系
	- Subtopic
	- LLE算法描述

- 算法描述

### 度量学习metric  learning

- 学习一个合适的距离度量
- Subtopic

## 特征选择与稀疏学习

### 子集搜索和评价

- 1.子集搜索(subset search)

	- 前向搜索('forword'):增加特征，逐渐增加特征直到学习器的效果变差
	- 后向搜索('backward'):从完整的特征集开始，每次尝试减少一个无关特征
	- 双向搜索('bidirectional'):每一轮增加特征，同时减少无关特征

- 2.子集评价(subset evaluation)

	- 信息增益

		- Subtopic

### 过滤式选择filter

- Relief

	- 步骤

		- 与xi最接近的同类样本，计算两者距离D(xi同类)
		- 与xi最接近的异类样本，计算两者距离D(xi异类)
		- 计算-D(xi同类)+D(xi异类)，遍历所有样本

			- Subtopic
			- Subtopic

		- 猜中临近与猜错临近属性j距离比较，调整

	- 应用法

		- 数值越大表示该特征的分类能力越强

### 包裹式选择wrapper

- LVW(las Vegas Wrapper)拉斯维加斯方法

	- 使用随机策略对特征集合进行搜索
	- 与蒙特卡洛方法比较

		- 有限时间：拉斯维加斯给出满足满足要求的解，或不给解；蒙特卡罗一定给出解，但未必满足要求
		- 无限时间：两者都能给出满足要求的解

	- 算法描述

- 与过滤式区别

	- 特征选择更好
	- 需要多次训练，开销大

### 嵌入式选择与L1正则化embedding

- 岭回归（L2）

	- Subtopic

- Lasso（L1） 可以选择特征

	- Subtopic

- 区别与联系

	- 都可以防止过拟合
	- L1更容易获得稀疏解，具有特征选择特征选择作用

### 稀疏表示与字典学习

- 将样本转化为合适的稀疏表示形式

### 压缩感知

- 奈奎斯特采样定理Nyquist

  采样频率达到模拟信号最高频率的两倍，采样后的信号就保留了模拟信号的全部信息

- 获得稀疏性s

	- 傅里叶变换
	- 小波变换
	- 余弦变换

- 限定等距性
- 矩阵补全

	- 应用于用户评分矩阵不全情况

## 计算学习理论

## 半监督学习

## 概率论图模型

## 规则学习

## 强化学习

## 参考
参考:[https://github.com/cjn-chen/machine_learn_reading_notes/tree/master/zhou_zhihua_minmap](https://github.com/cjn-chen/machine_learn_reading_notes/tree/master/zhou_zhihua_minmap)