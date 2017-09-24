---
layout: post
title: 构建微服务：Spring boot入门篇
categories: Linux
description: 构建微服务：Spring boot入门篇
keywords: Spring boot, 微服务
---
## 1. 什么是spring boot

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。它可以脱离tomcat等外部容器直接运行，快速构建微服务。

## 2. 使用spring boot有什么好处

平时如果我们需要搭建一个spring web项目的时候需要怎么做呢？

1. 配置web.xml，加载spring和spring mvc
2. 配置数据库连接、配置spring事务
3. 配置加载配置文件的读取，开启注解
4. 配置日志文件
...

配置完成之后部署tomcat 调试
...

现在非常流行微服务，如果我这个项目仅仅只是需要发送一个邮件,我都需要这样折腾一遍!
 
但是如果使用spring boot呢？
很简单，我仅仅只需要非常少的几个配置就可以迅速方便的搭建起来一套web项目或者是构建一个微服务！

## 3. 快速入门
### 3.1 maven构建项目

1、访问http://start.spring.io/

2、选择构建工具Maven Project、Spring Boot版本1.3.6以及一些工程基本信息，点击“Switch to the full version.”java版本选择1.7，可参考下图所示：
                                                                                             
![Alt text](https://github.com/gongenbo/gongenbo.github.io/raw/master/img/linux/20170924_springboot.png)    

