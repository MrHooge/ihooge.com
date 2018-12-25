---
title: Git 命令学习
date: 2017-04-11 10:20:38
tags: Git
---

# 在15分钟内学习Git
Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
## git init
初始化仓库
git将其文件和历史记录直接存储在项目中的文件夹中。要设置新的存储库，我们需要打开一个终端，导航到我们的项目目录并运行git init。这将为此特定文件夹启用Git，并创建一个隐藏的.git目录，其中将存储存储库历史记录和配置。

## git status
查看我们项目的当前状态
Git状态是另一个必须知道的命令，它返回有关存储库当前状态的信息：是最新的，全新的，发生了什么变化的等等。
<!--more-->
## git add [filename]
将文件添加到暂存区
Git有一个“分期区”的概念。你可以想像这样一个空白的画布，它保存你想提交的变化。它开始为空，但是您可以使用该git add命令向其添加文件（甚至单行和部分文件），最后提交所有内容（创建快照）git commit。

## git commit -m "desc"
这里列出的文件位于“暂存区”中，它们不在我们的存储库中。 我们可以在将它们存储在存储库中之前添加或删除仓库上的文件。
要存储我们的分段更改，我们运行commit命令，并附带一条描述我们已经更改的消息。
提交表示在给定时间点的存储库的状态。这就像一个快照，我们可以回头看看我们采用它的时间?

要创建一个新的提交，我们需要至少有一个更改添加到分段区域（我们只是使用git add）
该-m "desc commmit"部分是自定义用户编写的描述，总结了该提交中完成的更改。被认为是一个很好的做法，经常提交并总是写出有意义的提交信息。

## git log
我们有git log。想想Git的日志是一本记录，记录我们迄今为止所做的所有变化，按照我们承诺的顺序。

## git remote add origin https://github.com/xxxxx
把 文件添加到新仓库
项目可能同时拥有许多远程存储库。为了能够告诉他们，我们给他们不同的名字。传统上，git中的主要远程存储库称为origin。

## git push
push命令告诉Git我们准备好了我们的提交，现在我们准备好了。 所以我们把我们的地方变化推到我们的起始地位（在GitHub上）。

我们的远程的名称是origin，默认的本地分支名称是master。 -u告诉Git记住参数，所以下次我们可以简单的运行git push，Git会知道该怎么做。
现在是将我们的本地提交转移到服务器的时候了。这个过程称为推送，每次我们想要更新远程存储库时都会执行此操作。

执行此操作的Git命令是git push并且需要两个参数 - 远程repo的名称（我们称之为origin）和要推送的分支（master是每个repo的默认分支）。


## git pull 
更新项目

## git diff
用git diff命令与上次提交有什么不同。
diff的另一个很好的用途是查看已经提交的文件中的更改。 记住，分段文件是我们已经告诉git的文件，这些文件已经准备好被提交了。
使用--stage选项运行git diff，以查看刚刚上传的更改

## git reset
1：git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
2：git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
3：git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容

## git branch clean_up
创建 clean_up 分支

## git checkout <branch>
使用git checkout <branch>命令切换分支。

## git rm
使用git rm命令，它不仅可以从磁盘中删除实际的文件，还可以为我们移除文件。

## git merge
合并分支
## git branch -d <branch>
删除分支

## 配置Git

git config  - global user.name “My Name” 
git config  - global user.email myEmail@example.com

## git clone
此时，人们可以在Github上查看和浏览远程存储库。他们可以在本地下载并使用git clone命令完成工作副本

## 设置.gitignore

在大多数项目中，我们不想提交的文件或整个文件夹。我们可以git add -A通过创建.gitignore文件来确保它们不会被意外包含在我们的文件中：

手动创建名为.gitignore的文本文件，并将其保存在项目的目录中。
在里面，列出要忽略的文件/目录的名称，每一个都在一行。
必须添加，提交和推送.gitignore本身就像项目中的任何其他文件一样。


日志文件
任务运行程序构建
node.js项目中的node_modules文件夹
由Netbeans和IntelliJ等IDE创建的文件夹
个人开发人员笔记


