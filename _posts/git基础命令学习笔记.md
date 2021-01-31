---
title: Git基础命令学习笔记
url: git基础命令学习笔记
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - Git
tags:
  - Git
no-photos: >-
  https://random.52ecy.cn/randbg.php?size=1&rid-6686
date: 2020-11-10 22:16:00
---

git checkout master--切换到master分支

git merge branch1 把branch1分支合并到当前所在的分支

git commit 输入一个commit 提交到本地库中 

git push origin master 将本地master分支推送到远程上的origin服务器

* 当你想要将 master 分支推送到*origin*服务器时（再次说明，克隆时通常会自动帮你设置好那两个名字），那么运行这个命令就可以将你所做的备份到服务器

git fetch [remote-name]

git fetch master

它会从远程存储库下载更改，但不会将更改应用到您的代码中

这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

必须注意*git fetch*命令会将数据拉取到你的本地仓库——它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

git remote show [remote-name]

查看某个远程仓库

如果想要查看某一个远程仓库的更多信息，可以使用*

git remote show origin

它同样会列出远程仓库的 URL 与跟踪分支的信息

git pull*通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

自动的抓取然后合并远程分支到当前分支

Git冲突：commit your changes or stash them before you can merge.

有的时候使用git pull命令，可能遇到这样的问题：

Please, commit your changes or stash them before you can merge.  

Aborting  

这是由于远程库中的更改与本地的更改有冲突。

git的提示已经非常明确了，告诉我们要么把我们的更新进行commit要么就先stash本地更新。

第一种方法，stash：

那怎么stash本地的更新呢？直接执行：

git stash

git pull

git stash pop

接下来diff一下此文件看看自动合并的情况，并作出相应的修改。

git stash：备份当前的工作区，从最近一次提交中读取相关内容，让工作区保持和上一次提交的内容一致。同时，将工作区的内容保存到git栈中。

git stash pop：从git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个stash的内容，所以用栈来管理，pop会从最近一个stash中读取内容并恢复到工作区。

git stash list：显示git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。

git stash clear：情况git栈。

第二种方法：放弃本地的修改，直接覆盖

git reset --hard

git pull origin develop:develop

- [git book](https://git-scm.com/book/zh/v2)

- [git community book](http://gitbook.liuhui998.com/index.html)

git本地添加文件vi *.c git add git commit

//新建t1.c

touch t1.c 

//修改或新建t1.c

vim t1.c

git status

git add t1.c

git status

git commit -m '第一次提交'

git status

git本地修改文件vi *.c 

git add

//进入vim修改文件

vim t1.c

git status

git add t1.c

git本地删除文件git rm

rm -rf t1.c

![](cce4e589.png)

git rm t1.c

![](c4b2466a.png)

git status

Git - 分支的新建与合并

 https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6

![](19e548c9.png)
