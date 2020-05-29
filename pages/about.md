---
layout: page
title: About Me
description: 代码改变世界
keywords: 巩恩伯
comments: true
menu: 关于我
permalink: /about/
---
## 关于我

15年本科毕业，17年北漂，19年考上研究生，一个想到就去做的平凡的人。

## 联系我

* GitHub：[@gongenbo](https://github.com/gongenbo)
* 博客：[{{ site.title }}]({{ site.url }})
* 微博: [@巩大星](http://weibo.com/enbo)
* E-mail: gongenbo#buaa.edu.cn

## 技能关键词

#### 开发语言
<div class="btn-inline">
    {% for keyword in site.skill_language_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### 数据开发技能
<div class="btn-inline">
    {% for keyword in site.skill_data_science_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>
#### 机器学习
* Learning...