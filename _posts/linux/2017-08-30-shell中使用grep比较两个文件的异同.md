---
layout: post
title: shell中使用grep比较两个文件的异同
categories: linux
description: shell中使用grep比较两个文件的异同
keywords: sort, uniq
---
## 使用场景
今天遇到这样一个需求，有file1,file2两个文件，需要找出存在file2,但是不存在file1的id。
## 手动实现
自己用了循环实现，代码如下:`$ nohup sh diff.sh >/dev/null 2>&1 &`

```
#!/bin/sh
export LANG=en_US.UTF-8
. /etc/profile
. ~/.bashrc
cd $(dirname $0) || exit 1
sum=0
cat uid20170822.txt | while read line
do
	flag=0
	while read line2
	do
		if [ $line -eq $line2 ] ;then
                	flag=1
                	break
        	fi
	done < uid20170821.txt
	if [ $flag -eq 0 ];then
		echo $line >> diff.log
	fi
done
echo "finished!!!" >> diff.log
```
## 使用grep -vFf
统计file2中有，file1中没有的行,效果如上（但是不会去重）

```
$ grep -vFf file2 file1
```
## grep扩展
统计两个文本文件的相同行

```
grep -Ff file1 file2
```