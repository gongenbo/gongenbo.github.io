---
layout: post
title: Git常用命令整理
categories: [git]
description: Git常用命令整理
keywords: git,cheat,sheet
---

## 1.常用命令

git常用命令见下表：
![Alt text](https://gongenbo.github.io/img/git/git_cheat_sheet.jpg)


## 2.本地项目添加到github
首先在Github创建仓库，然后开始上传项目了,在项目的根目录下执行以下命令：

2.1 `git init` 初始化项目，执行完此命令后会生成一个.git文件夹 

2.2 `git add .` 将本地项目所有文件添加到git管理，.指全部文件 

2.3 `git commit -m "提交描述" `

2.4 `git remote add origin 刚刚新建的Github地址`   将本地项目与远程git仓库关联 

2.4 `git push -u origin master` 将本地项目推到远程仓库

执行此命令如果出现错误，应该是README.md文件在本地项目中不存在从而导致冲突，我的一贯解决办法就是用这个命令`git push -f origin master`，强制将本地项目push到远程仓库。在平常的操作中，用这个强制的命令很可能会出现很多问题，建议不要用，不过此处是初始化项目，实用这个命令就不会有什么问题了。 

如果有就先删除远程的github地址，命令：`git remote rm origin` 

git查看远程仓库地址: `git remote -v`


## 3. 删除远程文件夹

有时候误把.class文件或者.idea文件传上去了，只想删除远程仓库的文件，本地文件保留怎么操作呢？命令如下:

3.1 删除cached中的.idea文件

```git rm -r --cached .idea```	# --cached不会删除本地硬盘的文件夹

3.2 添加commit信息

```git commit -m "delete .idea directory"```

3.3 推送到远程库

git push -u origin master

备注：如果想每次提交都不增加.idea文件夹，可以新建一个.gitignore文件，这样上传的时候就会自动忽略了，文件内容如下：
```$xslt
git # Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

#idea
.idea/*
target/*
*.iml

```

## 4.将master合并到dev分支

这种操作适用于版本更新迭代比较快，其他人已经将代码合并到了master，自己再分支上也开发了另一个功能，需要同步下来master最新的代码进行测试，步骤如下：

4.1 将分支切换到master，合并完成后可以通过`git branch`查看当前所在的分支

```git checkout master```

4.2 将代码pull到本地

```git pull origin master```

4.3 切换到你所在分支dev

```git checkout dev```

4.4 使用merge命令将master合并到dev分支

```git merge master```

4.5 将本地内容push到dev分支

```git push origin dev:dev```

## 5.将dev合并到master

最简单的情况是通过github的可视化界面，直接点merge按钮提交request合并到master，但是有时候需要本地先将dev合并到master进行测试，此时可以使用如下命令：

5.1 首先切换到master

```git checkout master```

5.2 使用merge命令将dev合并进master分支

```git merge dev```

此时本地的master已经是合并完dev分支最新的代码了，可以直接打包测试了。如果测试没问题的话并且你有推到master的权限的话，也可以通过`git commit -m "描述"`,`git push origin master`直接推送到master分支。

## 6.强制覆盖本地代码
如果本地修改错了，想强制覆盖本地代码（与git远程仓库保持一致），步骤如下
### 常规方法
6.1 拉取所有更新，不同步；
```$xslt
git fetch --all
```

6.2 本地代码同步线上最新版本(会覆盖本地所有与远程仓库上同名的文件)；
```$xslt
git reset --hard origin/master
```
6.3 再更新一次（其实也可以不用，第二步命令做过了其实）
```$xslt
git pull
```
### 简洁方法

这里我常用的简洁命令是`git reset --hard HEAD`，在Git中，用HEAD表示当前版本，，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
