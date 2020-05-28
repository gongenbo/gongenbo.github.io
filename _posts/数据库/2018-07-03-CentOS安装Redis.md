---
layout: post
title: CentOS安装Redis
categories: [数据库]
description: Redis安装
keywords: CentOS,redis
---
## 
#### 1.安装Redis
最快捷的方法使用yum安装

```
# yum install redis
```
#### 2.修改配置文件
安装完后如果想从别的机器访问redis需要设置redis.conf文件

```
vi /etc/redis.conf
```
```
# bind 127.0.0.1  需要注释掉。因为这个是设置redis服务器只在本地监听，从而会拒绝来自外网的监听
protected-mode no  
daemonize yes  设置为后台启动
```
#### 3.redis常用命令
- 指定配置文件启动
```
redis-server /etc/redis.conf
```
- 关闭redis
```
redis-cli shutdown
```
- 本地登陆
```
redis-cli
```
- 从别的机器远程登录
```
redis-cli -h 172.17.*.* -p 6379 -a redis
```

