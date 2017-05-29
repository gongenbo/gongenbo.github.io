---
layout: post
title: 使用shell的nologin禁止用户登录
categories: Linux
description: 使用shell的nologin禁止用户登录
keywords: keyword1, keyword2
---

# 使用场景
既不想删除用户，也不想删除用户的目录权限，只是想禁止用户登录，如果用户一登录就logout退出并提示相关信息。
# 设置方法
使用sudo权限修改/etc/passwd文件中用户后面的/bin/bash为/sbin/nologin。
    vi /etc/passwd
新建 /etc/nologin.txt 这个文件，在文件内面写上不能登陆的原因，当用户登录时，屏幕上就会出现这个文件里面的内容。