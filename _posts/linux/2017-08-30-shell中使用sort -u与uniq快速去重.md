---
layout: post
title: shell中使用sort -u与uniq快速去重
categories: linux
description: shell中使用sort -u与uniq快速去重
keywords: sort, uniq
---
## 使用场景
一个文本中很多人名或订单号重复，实现快速去重
## sort -u
### 简单应用
sort -u 的原理是先排序，然后将相邻的重复元素合并删除。

简单例子
`$ cat test`

```              
jason
jason
jason
fffff
jason

```
使用sort -u 去重命令：
`$ sort -u test`

结果：

```
fffff
jason
```
### 高深应用

## uniq
### 简单应用
使用uniq去重是合并连续出现的相同记录

使用uniq去重命令`$uniq test  `

结果：

```
fffff
jason
```