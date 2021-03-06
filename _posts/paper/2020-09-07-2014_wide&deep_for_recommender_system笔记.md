---
layout: post
title: DLRS2016 Wide & Deep Learning for Recommender System笔记
categories: [paper]
description: DLRS2016 Wide & Deep Learning for Recommender System笔记
keywords: DLRS,wide,deep,Facebook,predict,rank,笔记,翻译
---

# 论文

论文原址：[Wide & Deep Learning for Recommender Systems](https://arxiv.org/abs/1606.07792)

# 摘要
Wide & Deep 模型的核心思想是结合线性模型的记忆能力和 DNN 模型的泛化能力，从而提升整体模型性能。Wide & Deep 已成功应用到了 Google Play 的app推荐业务，并于TensorFlow中封装。该结构被提出后即引起热捧，在业界影响力非常大，很多公司纷纷仿照该结构并成功应用于自身的推荐等相关业务。

**DLRS** 全称 Workshop on Deep Learning for Recommender Systems，2016举办第一届，旨在加速深度学习在RecSys社区中的传播和应用。深度学习已经在很多领域上取得巨大成功，会议组织相信它有潜力成为下一代推荐系统的核心，现在已经是将它们应用于RecSys的时候了。

# 1. 介绍

推荐系统可以被看作为搜索排名系统，其中输出是一组用户和上下文的信息，用户和上下文信息，输出是每一项的排名列表。给定查询，建议任务是查找数据库中的相关项目，然后根据特定目标（如点击或购买）对项目进行排名。
 
推荐系统的一个挑战，和一般的搜索排名系统类似，是实现两者 **memorization** 和 **generalization**。

**Memorization** 可以粗略地定义为学习项目或特征的频繁共发生，并利用（**exploiting**）历史数据中可用的关联。基于 Memorization 的推荐通常更热门，并且与用户已经执行的操作的项目直接相关。

**Generalization** 基于相关性的传递性，并探索（**explores**）了新的特征组合，过去从未或很少发生。与 Memorization 相比，Generalization 倾向于提高推荐项目的多样性。

**交叉特征**通常有两种生成方式：

**记忆式（memorization）** 根据历史数据当中不同特征同时出现的频率来学习交叉特征，常见的做法是对大量的特征做 cross-product 的特征转换，这种方式有效且具备可解释性；

**生成式（generalization）** 生成式的方法是基于相关性可传递的性质，挖掘出一些深层难以发现的特征组合，常见的方式有两种方式：

- 人工：需要具备专业的业务领域知识和大量的特征工程的操作；

- DNN：将高维稀疏的输入转化为低维稠密的embeddings再通过DNN进行训练，可以挖掘更深层的交叉特征；采用这种方法的主要缺陷是在输入过于稀疏时有可能出现类似于欠拟合的情况而推荐一些不相关的物品；

WDL将一个基于cross-product的线性模型（单纯的cross product只能学习到用户有过的行为）和一个基于embeddings（FM/NN都是基于embedding的，可以学习到不易发现的交叉组合）的前馈神经网络模型结合到一起进行联合训练，也就相当于结合了记忆式和生成式两种方法来生成交叉特征，再对生成的交叉特征进行建模学习。

1.1 广义线性模型(generalized linear models)

对于在产业化环境中的大规模的在线推荐和排名系统，广义线性模型（如逻辑回归）被广泛使用，因为它们简单、可扩展且可解释。

模型通常使用 one-hot encoding 在二进制的稀疏特征上进行训练。

例如： 用户安装 Netflix ，则二进制功能 user_installed_app_netflix 具有值 1。Memorization 可以通过跨产品对稀疏的特征如 AND(user_installed_app=netflix, impression_app=pandora”) 的转换来实现。这个解释了如何通过目标标签对匹配同时出现的特征值。

1.2 广义化 Generalization

广义化 Generalization 可以通过粒度较低的特征值来添加，例如 AND(user_installed_**category**=video, impression_category=music)，但是通常需要手动的特征工程。跨产品转换的一个限制是它们不概括到未出现在训练数据中的查询项的特征对。

1.3 嵌入式的模型(Embedding-based models)

嵌入式的模型，例如因子分解机（FM）或深度神经网络，可以通过学习每个查询和项特征的低维密集嵌入向量(dense embedding vector)，可以泛化到之前未见过的query-item特征对，从而减特征值处理的负担。

但是，当基础查询项矩阵稀疏且排名较高（例如具有特定偏好的用户或吸引力较小的合适项目）时，很难学习查询和项的有效低维表示形式。在这种情况下，大多数查询项对之间不应有交互，但密集嵌入将导致对所有查询项对进行非零预测，从而可以过度概括和提出不太相关的建议。另一方面，具有跨产品特征转换的线性模型可以记住这些” 异常规则”，而参数要少得多。

![Alt text](/img/paper/wide_deep/figure1.png)
本论文通过如下所示的 Figure 1（该论文主要的贡献） 联合训练线性模型组件和神经网络组件，在一个wide & deep学习框架中通过一个模型实现 memorization 记忆化 和 generalization 广泛化 。

该文件的主要贡献包括：

- wide & deep 学习框架，用于共同训练线性模型和具有稀疏输入的通用推荐系统（generic recommender systems）的特征转换。
- Wide＆Deep 推荐系统的实施和评估是在 Google Play（一家拥有超过 10 亿活跃用户和超过 100 万个应用程序的移动应用程序商店）上生产的。
- 我们在 TensorFlow 中将我们的实现以及高级 API 开源了

具有嵌入和的前馈神经网络虽然想法很简单，但我们证明 Wide＆Deep 框架显着提高了移动应用商店中的应用获取率，同时满足了培训和服务速度方面的要求。


# 2. 推荐系统总览
![Alt text](/img/paper/wide_deep/figure2.png)

应用程序推荐系统的概述如 Figure 2 所示。当用户访问应用程序商店时，将生成一个查询，其中可能包含各种用户和上下文功能。 推荐系统返回一个应用程序列表（也称为展示次数），用户可以在其上执行某些操作，例如点击或购买。 这些用户操作以及查询和印象将作为学习者的训练数据记录在日志中。

由于数据库中有超过一百万个应用程序，因此在服务等待时间要求（通常为10毫秒）内为每个查询详尽地为每个应用程序评分是很困难的。 因此，接收到查询的第一步是检索。 检索系统使用各种信号（通常是机器学习的模型和人工定义的规则的组合）返回简短匹配的项目清单。 减少候选库后，排名系统将所有项目按其得分进行排名。 分数通常为 `P（y | x）`，带有特征 x 的用户动作的概率，包括用户功能（例如，国家 / 地区，语言，人口统计信息），上下文功能（例如，设备，一天中的小时，星期几）以及 印象功能（例如，应用程序年龄，应用程序的历史统计信息）。 在本文中，我们重点研究使用wide & deep学习框架的排名模型。

# 3. wide & deep学习


## 3.1 wide 组件

The wide component 是形式 **y = wTx + b** 的广义线性模型，如图 1（左）所示。y是预测，x = [x1，x2，…，xd] 是特征的向量，共有d个维度，w = [w1， w2，…，wd] 是模型参数，并且是偏差。特征集包括原始输入特征和变换特征。 最重要的转换之一是跨产品转换，其定义为：

![Alt text](/img/paper/wide_deep/formula1.png)

其中 cki 是一个布尔变量，如果第 i 个特征是第 k 个变换 φk 的一部分，若某个维度是one hot encoding的向量，则为 1，否则为 0。对于二进制特征，请进行叉积变换（例如，“AND（gender = female，language = en ）”）仅当构成要素（“性别 = 女性” 和 “语言 = zh-CN”）全部为 1 时为 1，否则为 0。 这捕获了二元特征之间的相互作用，并为广义线性模型增加了非线性。


## 3.2 deep 组件
The deep component 是前馈神经网络，Deep主要包括三层：input layer, embedding layer和hidden layer，hidden layer的结果输出到output layer;

如图 1（右）所示，对于分类要素，原始输入是要素字符串（例如，“language = en”）。 

- 首先将这些稀疏的高维分类特征（Sparse Features）转换为低维且密集的实值向量，通常将其称为嵌入向量(Dense Embedding)。 嵌入的维度通常约为 O（10）到 O（100）。 
- 随机初始化嵌入向量，然后训练值以最小化模型训练期间的 finalloss 函数。 然后将这些低维密集嵌入向量馈入神经网络的隐藏层中。

具体而言，每个隐藏层执行以下计算：

![Alt text](/img/paper/wide_deep/formula2.png)                                    

其中层号和激活函数通常为线性整流单元（ReLU）。a（l），b（l）和 W（l）是第 1 层的激活，偏差和模型权重。 

## 3.3 wide & deep 联合训练

Wide组件和Deep组件组合在一起，对它们的输入日志进行一个加权求和来做为预测，它会被feed给一个常见的logistic loss function来进行联合训练。注意，联合训练（joint training）和集成训练（ensemble）有明显的区别。在ensemble中，每个独立的模型会单独训练，相互并不知道，只有在预测时会组合在一起。相反地，**联合训练（joint training）会同时优化所有参数，通过将wide组件和deep组件在训练时进行加权求和的方式进行**。这也暗示了模型的size：对于一个ensemble，由于训练是不联合的（disjoint），每个单独的模型size通常需要更大些（例如：更多的特征和转换）来达到合理的精度。**相比之下，对于联合训练（joint training）来说，wide组件只需要补充deep组件的缺点，使用一小部分的cross-product特征转换即可，而非使用一个full-size的wide模型**。

WDL的wide和deep部分同时训练，对梯度进行后向传播算法、采用 mini batch SGD 调节参数值， 但是wide和deep两部分的优化器选择不同，wide部分选用 **L1正则化的[FTRL](https://zhuanlan.zhihu.com/p/32903540)** 的优化器， deep部分选用**AdaGrad**优化器。

对于逻辑回归问题，模型的预测为：

![Alt text](/img/paper/wide_deep/formula3.png)  

其中 Y 是二进制类标签，σ（・）是 S 形函数，φ（x）是原始特征 x 的叉积变换，，并且 b 是偏差项。wwide 是所有模型权重的向量，和 wdeep 是施加在最终激活 a（lf）上的权重。


# 4. 系统实现

![Alt text](/img/paper/wide_deep/figure3.png)

应用程序推荐传递途径的实现包括三个阶段：数据生成，模型训练和模型服务，如 Figure 3 所示。

## 4.1 数据生成

在此阶段，一段时间内的用户和应用曝光数据将用于生成训练数据。 每个示例对应一个曝光。 标签为 app acquisition：如果此程序被安装，则为 1；否则为 0。

在此阶段还会生成词汇表，这些表是将**字符串映射为整数 ID** 的表。系统会为所有出现最少次数的字符串特征计算 ID 空间。 通过将特征值 x 映射到其累积分布函数 P（X≤x）（除以 intonqquantiles），可以将 **连续的实值** 特征 **标准化** 为 [0,1]。 第 i 个分位数中的值的归一化值。 在数据生成期间计算分位数边界。

## 4.2 模型训练

我们在实验中使用的模型结构如图 4 所示。下图是用于app推荐的WDL模型结构。 wide部分是用户想要的app和用户安装的app之间的交叉特征； **deep部分将每个的离散属性转化为32维的embeddings，并和所有的连续特征整合到一起，共计约1200维，然后输入到3层ReLU的隐藏层**； output是一个sigmoid函数；

![Alt text](/img/paper/wide_deep/figure4.png)

wide 和 deep 联合模型已针对超过 5,000 亿个示例进行了训练。 每次收到一组新的训练数据时，都需要对模型进行重新训练。 但是，每次从头开始的重新训练在计算上都是昂贵的，并且会延迟从数据到达到提供更新模型的时间。 为了应对这一挑战，我们实施了热启动系统，该系统使用嵌入的模型和先前模型的线性模型权重来初始化新模型。

在将模型加载到模型服务器之前，需要对模型进行空运行以确保它不会在提供实时流量方面引起问题。 我们根据以前的模型经验性地验证模型质量，以进行完整性检查。

## 4.3 模型服务

对模型进行训练和验证后，我们会将其加载到模型服务器中。 对于每个请求，服务器从应用程序检索系统和用户功能部件接收一组应用程序候选者，以对每个应用程序进行评分。 然后，从最高得分到最低得分对应用程序进行排名，然后按顺序将这些应用程序显示给用户。 通过在 Wide＆Deep 模型上运行前向推理过程来计算分数。

为了满足 10 毫秒量级的每个请求，我们通过多线程并行运行来优化性能，方法是并行运行较小的批处理，而不是在单个批处理推理步骤中对所有候选应用程序评分。

# 5. 实验结果

为了在真实的推荐系统上评估Wide & Deep learning的效果，我们运行了在线试验，并在两部分进行系统评测：app获得率（app acquisitions）和serving性能。

## 5.1 应用获得率

我们在 A / B 测试框架中进行了 3 周的在线实时实验。 对于对照组，随机选择了 1％的用户，并向其提供由先前版本的排名模型生成的推荐，该排名模型是高度优化的全范围逻辑回归模型，具有丰富的跨产品特征变换。 对于实验组，向 1％的用户展示了由 Wide＆Deep 模型产生的建议，并接受了相同的功能集培训。 如表 1 所示，

![Alt text](/img/paper/wide_deep/table1.png)

Wide＆Deep 模型相对于对照组（具有统计学意义）将应用商店主登陆页面上的应用获取率提高了 + 3.9％。 还仅使用具有相同特征和神经网络结构的模型的深层部分，将结果与另一个 1％的小组进行了比较，宽和深层模式在仅深层模型之上具有 + 1％的增益（具有统计意义） 。

除了在线实验，我们还显示了接收器操作员区域下的特征
脱机设置的保持线上的曲线（AUC）。 虽然 Wide＆Deep 的离线 AUC 略高，但对在线流量的影响更大。 一种可能的原因是，脱机数据集中的印象和标签是固定的，而在线系统可以通过将概括与记忆相结合来生成新的探索性建议，并从新的用户响应中学习。

## 5.2 服务表现

商业移动应用商店面临高流量，以高吞吐量和低延迟提供服务是一项挑战。 在流量高峰时，我们的推荐服务器每秒可记录超过 1000 万个应用程序。 使用单线程时，对单个批处理中的所有候选者评分需要 31 毫秒。 我们实施了多线程，并将每个批处理分成较小的大小，这将客户端延迟显着减少到 14 ms（包括服务开销），如表 2 所示。

![Alt text](/img/paper/wide_deep/table2.png)

# 6. 相关工作

将使用特征交叉转换的wide linear model与使用dense embeddings的deep neural networks，受之前工作的启发，比如FM：它会向线性模型中添加generalization，它会将两个变量的交互分解成两个低维向量的点积。在该paper中，我们扩展了模型的能力，通过神经网络而非点积，来学习在embeddings间的高度非线性交叉(highly nonlinear interactions)。

在语言模型中，提出了RNN和n-gram的最大熵模型的joint training，通过学习从input到output之间的直接权重，可以极大减小RNN的复杂度（hidden layer）。在计算机视觉中，deep residual learning被用于减小训练更深模型的难度，使用简短连接（跳过一或多层）来提升accuracy。神经网络与图模型的joint training被用于人体姿式识别。在本文中，我们会探索前馈神经网络和线性模型的joint training，将稀疏特征和output unit进行直接连接，使用稀疏input数据来进行通用的推荐和ranking问题。

# 总结

## Memorization
之前大规模稀疏输入的处理是：通过线性模型 + 特征交叉。通过特征交叉能够带来很好的效果并且可解释性强。但是 Generalization（泛化能力）需要更多的人工特征工程。
## Generalization
相比之下，DNN 几乎不需要特征工程。通过对低纬度的 dense embedding 进行组合可以学习到更深层次的隐藏特征。但是，缺点是有点 over-generalize（过度泛化）。推荐系统中表现为：会给用户推荐不是那么相关的物品，尤其是 user-item 矩阵比较稀疏并且是 high-rank（高秩矩阵）

# 代码
[TensorFlow官方实现代码](https://github.com/tensorflow/models/tree/master/official/wide_deep)

# 参考
- [「Wide & Deep Learning for Recommender Systems」- 翻译](https://learnku.com/articles/40434)
- [CTR - wide & deep Model](https://betterxys.github.io/2017/09/29/widedeepmodel/)
- [知乎 详解 Wide & Deep 结构背后的动机](https://zhuanlan.zhihu.com/p/37733208)
- [基于Wide & Deep Learning的推荐系统](http://d0evi1.com/widedeep/)