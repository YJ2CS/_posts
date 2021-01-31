---
title: Git 本地和远程分支
url: git-本地和远程分支
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-7cdd'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-23T00:19:10+08:00'

---

如果想把本地的某个分支test提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，那么可以这么做。

```git
$ git push origin test:master         // 提交本地test分支作为远程的master分支 //好像只写这一句，远程的github就会自动创建一个test分支
//!!! 远程的master分支会被完全删除，然后将本地test分支作为远程的master分支

```

需要注意，这种情况下远程的master分支经历两个操作，先被完全删除，然后被本地的test分支替代，即完全为dev分支的内容

```git
$ git push origin test:test              // 提交本地test分支作为远程的test分支
```

更常用的命令是将在一个分支所做的工作fetch到主分支（master）中，即程序员1将他做的工作(分支test1)合并到了master，而程序员2程序员1将他做的工作(分支test2)也合并到了master
两者可以同时做不同的两个任务，比如一位在开发后端的管理系统，另外一位在给前端画布局

git fetch是从远程存储库检索数据。//这些数据下载下来后并不会立刻覆盖你现在的工作

git merge是合并来自多个工作行的工作（通常是本地分支）。比如把fetch下来的数据合并到你现在的工作

git pull : 首先，基于本地的FETCH_HEAD记录，比对本地的FETCH_HEAD记录与远程仓库的版本号，然后git fetch 获得当前指向的远程分支的后续版本的数据，然后再利用git merge将其与本地的当前分支合并。所以可以认为git pull是git fetch和git merge两个步骤的结合。

pull=fetch+merge
**需要注意，pull -f 会强制覆盖**
你可以使用"跟踪信息"确定这一点。("tracking information" determines this.)
