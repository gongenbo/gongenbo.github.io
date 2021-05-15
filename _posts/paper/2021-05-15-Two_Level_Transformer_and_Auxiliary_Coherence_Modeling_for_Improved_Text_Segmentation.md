---
layout: post
title: 双层Transformer与附加连贯性建模用于提高文本分割论文笔记
categories: [paper]
description: 双层Transformer与附加连贯性建模用于提高文本分割论文笔记
keywords: Transformer,Text Segmentation
---

# 论文
[Two-Level Transformer and Auxiliary Coherence Modeling for Improved Text](https://arxiv.org/abs/2001.00891)

# 0. 为什么要读这篇文章

最近有一个作文切题度任务，希望能借助这篇论文的思想，先使用本文双层Transformer的思想进行文本分割，然后再进行相似度对比。

# 1. 简介

文本的连贯性与文本的分割具有本质上的紧密联系，在分割后，同一部分的文本之间的连贯性应当强于不同部分的文本。如下图，共四种分割方式，其中T1均描述荷兰历史，T2均描述荷兰地理，T3,T4包含两个部分的句子。且T1的连贯性强于T3和T4，这说明第四句应该是一个新部分的起始。

![片段说明文本一致性与分割的关系](/img/paper/cats/1.png)

依据语义连贯性解构长文本能够提升文本的可读性，有益于下游任务(检索、摘要、主题分类等)。而现存的文本分割方法都仅仅隐式的使用连贯性这项特征。

本文提出的监督模型使用双层Transformer网络编码句子序列，除了以预测分割结果作为目标以外，还对连贯性进行建模并应用在训练过程中。迫使模型预测连贯性更强的文本段落作为分割结果。进一步，文章提出的模型能够实现零样本语言迁移，其使用英文维基百科训练的模型能够在其他语言上获得较好的分割效果。

# 2. CATS: 连贯性感知双层Transformer用于文本分割

下图2就是这个CATS（Coherence-Aware Two-Level Transformer for Text Segmentation）模型
![cats](/img/paper/cats/2.png)
模型接收定长句子序列作为输入，字符编码由预训练词嵌入和位置嵌入连接得到。句子序列首先通过字符级Transformer得到句子表示，再经句子级Transformer强化上下文信息。编码器输出经过一前馈二分类器对各句进行分类。另将编码后的句子序列输入另一前馈网络获取一个连贯性分数。
获取位置嵌入的方法：[Transformer 中的 positional embedding](https://zhuanlan.zhihu.com/p/359366717)

## 2.1 以Transformer为基础的文本分割

选取分层结构的原因在于，某个句子属于哪一部分不单取决于其内容，还取决于其所处的上下文语境。选取Transformer则是因为该结构近期在NLP领域中的广泛应用和优于传统循环网络的优越性。

### 2.1.1 句子编码

定义

句子序列：![juzixulie](/img/paper/cats/2_1_1_1.png) 句子数 ：K

句子：![juzixulie](/img/paper/cats/2_1_1_2.png) 句长：T

句首添加特殊标识符 ![juzixulie](/img/paper/cats/2_1_1_3.png) 用于在经过低层后获取句子的表示

[公式] 由预训练词嵌入和位置嵌入组合而成， $${\rm Transform}_T$$ 表示一层Transformer，则对于第一层，有

取结果序列首位作为句子表示 [公式]

### 2.1.2 句子置于上下文语境中

### 2.1.3 分割分类

## 2.2 附加连贯性模型

## 2.3 创建训练实体

## 2.4 推理

## 2.5 利用跨语言词嵌入实现零样本语言迁移

# 3. 实验装置

主要数据集：WIKI-727K Corpus、Standard Test Corpora、以及少部分非英语数据

评价指标： [公式] 分数，即任取一段由k各句子组成的序列，判断该序列的首句和尾句是否处于不同分段中，该分值越大，代表分割的效果越差。实验过程中，k取正确段落平均句子数的一半。

对比基线模型：1)有监督文本分割模型Koshorek et al. (2018） 2)无监督文本分割模型 GRAPHSEG 3)按比例随机给予正标签的RANDOM测试，为 [公式] 提供上界

消融实验：对比了有无显式连贯性建模的模型CATS(有)和TLT-TS(无) 

# 3. 实验装置

# 4. 结果分析
# 4.1 基础评估

# 4.2. 语言迁移

结果表明，具有辅助连贯性建模的模型CATS的表现优于其余模型。尽管相对于在英文数据集上的结果，语言迁移仍导致了性能的部分下降，但该结果还是给缺乏大量正确分割结果的数据集的处理提供了可能。
[juzixulie](/img/paper/cats/2_1_1_1.png) 

# 5. 结论

现存文本分割模型多依据相邻句子之间词汇或词义的重复率来表征文段的连贯性，以此作为分割的依据。本文提出由两层Transformer构成的监督模型，低层编码句子，高层编码句子序列。在两项任务上进行训练，分别为 1)预测句子分段标签 2)对比正确分段和错误分段的连贯性。该模型在多个文本分割数据集上取得sota（state-of-the-art，指该项研究任务中，最好/最先进的模型），且能够实现向其他语言的零样本迁移。未来或可在负样本构建的过程上进行改进。

# 代码实现
[Github EducationalTestingService/CATS](https://github.com/EducationalTestingService/CATS)

我在使用这个代码跑模型的时候发现缺少词嵌入与预训练的数据集，尝试使用[Fasttext](https://fasttext.cc/docs/en/pretrained-vectors.html)作为预训练的en.vectors但是加载报错，有跑通的希望能喊我一声：gongenbo@gmail.com。

# 参考
- [知乎 《双层Transformer辅以连贯性建模用于文本分割》阅读笔记](https://zhuanlan.zhihu.com/p/108775768)
