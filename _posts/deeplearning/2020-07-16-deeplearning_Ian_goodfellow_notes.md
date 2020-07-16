---
layout: post
title: 《深度学习》Ian Goodfellow笔记
categories: [machinelearning]
description: 深度学习笔记
keywords: Ian Goodfellow,深度学习,笔记
---

# 深度学习——Ian Goodfellow

## 简介
本文整理了Ian Goodfellow《深度学习》花书自己觉得有收获的点


## 1.引言
深度学习分可见层（输入图像）、第1隐藏层隐藏层（边）、第2隐藏层（角和轮廓）、第3隐藏层（对象部分）、输出，隐藏层越深特征越抽象。

基于感知机（单层网络二分类器）和ADALINE（自适应线性单元）中使用的函数f(x,w)的模型称为线性模型，线性模型有很多局限，无法学习异或（XOR）函
数，即f([0,1],w)=1和f([1,0],w)=1,但f([1,1],w)=0和f([0,0],w)=0。第一次热潮衰退。

## 2.线性代数
### 2.1 标量、向量、矩阵和张量
张量：坐标超过两维的数组

矩阵和向量相加，也就是说向量b与矩阵A的每一行相加

$$
 \left[
 \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right] \tag{3}
$$ + 
### 2.2 矩阵和向量相乘


## 参考
《深度学习》 Ian Goodfellow