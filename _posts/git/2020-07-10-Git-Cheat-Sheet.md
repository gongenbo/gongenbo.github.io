---
layout: post
title: Git常用命令整理
categories: [git]
description: Git常用命令整理
keywords: git,cheat,sheet
---

## 1.常用命令

git常用命令见下表：
![Alt text](https://github.com/gongenbo/gongenbo.github.io/raw/master/img/git/git_cheat_sheet.jpg)


## 2.本地项目添加到github
- 首先在Github创建仓库，然后开始上传项目了 
- 在项目的根目录下执行以下命令：
1. `git init` 初始化项目，执行完此命令后会生成一个.git文件夹 
2. `git add .` 将本地项目所有文件添加到git管理，.指全部文件 
3. `git commit -m "提交描述" `
4. `git remote add origin 刚刚新建的Github地址`   将本地项目与远程git仓库关联 
5. `git push -u origin master` 将本地项目推到远程仓库
- 执行此命令如果出现错误，应该是README.md文件在本地项目中不存在从而导致冲突，我的一贯解决办法就是用这个命令`git push -f origin master`，强制将本地项目push到远程仓库。在平常的操作中，用这个强制的命令很可能会出现很多问题，建议不要用，不过此处是初始化项目，实用这个命令就不会有什么问题了。 
- 如果有就先删除远程的github地址，命令：`git remote rm origin` 
- git查看远程仓库地址: `git remote -v`


## 3. 删除远程文件夹
- 有时候误把.class文件或者.idea文件传上去了，只想删除远程仓库的文件，本地文件保留怎么操作呢？命令如下:

## 4.将master合并到dev分支
- 这种操作适用于版本更新迭代比较快，其他人已经将代码合并到了master，自己再分支上也开发了另一个功能，需要同步下来master最新的代码进行测试，步骤如下：

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
- 最简单的情况是通过github的可视化界面，直接点merge按钮提交request合并到master，但是有时候需要本地先将dev合并到master进行测试，此时可以使用如下命令：

5.1 首先切换到master

```git checkout master```

5.2 使用merge命令将dev合并进master分支

```git merge dev```

- 此时本地的master已经是合并完dev分支最新的代码了，可以直接打包测试了。如果测试没问题的话并且你有推到master的权限的话，也可以通过`git commit -m "描述"`,`git push origin master`直接推送到master分支。