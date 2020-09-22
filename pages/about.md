---
layout: page
title: About Me
description: Better late than never
keywords: 巩恩伯
comments: true
menu: 关于我
permalink: /about/
---

## 联系我

* GitHub：[@gongenbo](https://github.com/gongenbo)
* 博客：[{{ site.title }}]({{ site.url }})
* 微博: [@巩大星](http://weibo.com/enbo)
* E-mail: gongenbo#gmail.com

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
<div class="btn-inline">
    {% for keyword in site.skill_machine_learning_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>