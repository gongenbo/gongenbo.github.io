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
```
import numpy as np
x = np.array([[1, 2, 3], [4, 5, 6]])
z = np.array([1, 2, 3])
print(x+z)
#output
[[2 4 6]
 [5 7 9]]
```

### 2.2 矩阵和向量相乘

A矩阵的列必须与B矩阵行相同

结论：
1. 元素乘法：np.multiply(a,b)
2. 矩阵乘法：np.dot(a,b) 或 np.matmul(a,b) 或 a.dot(b) 或直接用 a @ b !
3. 唯独注意：*，在 np.array 中重载为元素乘法，在 np.matrix 中重载为矩阵乘法!

```
x = np.array([[1, 2, 3], [4, 5, 6]])
z = np.array([[1, 2], [2, 3], [4, 5]])
print(np.dot(x, z))
x = np.array([[1, 2, 3], [4, 5, 6]])
z = np.array([[1, 2, 3], [2, 3, 4]])
print(np.multiply(x, z))
print(x*z)
```
输出
```
[[17 23]
 [38 53]]
[[ 1  4  9]
 [ 8 15 24]]
[[ 1  4  9]
 [ 8 15 24]]
```

逆矩阵求解方法：[https://www.shuxuele.com/algebra/matrix-inverse-minors-cofactors-adjugate.html](https://www.shuxuele.com/algebra/matrix-inverse-minors-cofactors-adjugate.html)
### 2.5 范数
范数是将向量映射到非负值的函数，向量x的范数衡量从原点到点x的距离，公式如下：

${|\left|x\right||}_p={(\sum_{i}{|x_i|}_p)}^\frac{1}{p}$

## 参考
《深度学习》 Ian Goodfellow