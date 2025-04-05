---
layout: post
title: GitHub博客markdown文章插入LateX公式
categories: [markdown]
description: GitHub博客markdown文章插入LateX公式
keywords: Github,博客,markdown,公式,LateX,MathJax
---

## 问题简介
通过MathJax渲染LateX公式在markdown中显示

以前Github博客markdown文章中插入公式都是用word写好公式，然后截图放入github page的文件夹，然后通过
`![Alt text]({{site.url}}/img/database/20171016_mysql1.png)`输出图片的形式显示公式的。

能不能通过Late形式输出公示，搜集了网上资料可以通过MathJax渲染在markdown输出LateX公式。


## 实现方式
### 添加MathJax渲染
首先在github page的_includes/header.html的head中增加MathJax渲染Latex公式的引擎，代码如下
```
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
    </script>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```

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

公式两边需要加$或$$，粘贴到markdown中，最后公式效果如下：

$${|\left|x\right||}_p={(\sum_{i}{|x_i|}_p)}^\frac{1}{p}$$

$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$

## github page中latex公式渲染
### 方法1：直接拷贝
右键点击公式选择“复制到剪贴板”，TeX命令，然后在公式两边加上双$$号

$$\theta^{*}=Q(a^{*})=\max_{a \in \mathcal{A}} Q(a) = \max_{1 \leq i \leq K} \theta_i$$


$$Q(a) = \mathbb{E}[r|a] = \theta$$


### 方法2：使用在线转换工具
1. [在线HTML 转换为 Markdown](https://devtool.tech/html-md)
2. chatgpt转中文，复制到markdown或者typora保存

## 参考
[How to support latex in github-pages?](https://stackoverflow.com/questions/26275645/how-to-support-latex-in-github-pages)

[如何超迅速地把Word的公式转换成LaTeX公式](https://zhuanlan.zhihu.com/p/32321996)
