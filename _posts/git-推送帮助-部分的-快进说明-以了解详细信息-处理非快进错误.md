---
title: git 推送帮助”部分的“快进说明”以了解详细信息-处理非快进错误
siteurl: git-推送帮助-部分的-快进说明-以了解详细信息-处理非快进错误
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-329c'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-23T00:19:26+08:00'

---

# 处理非快进错误

有时，Git 无法在不丢失提交的情况下对远程仓库进行更改。 发生此情况时，推送会被拒绝。

如果其他人已推送与您相同的分支，Git 将无法推送您的更改：

```bazaar
$ git push origin master
> To https://github.com/USERNAME/REPOSITORY.git
>  ! [rejected]        master -> master（非快进）
> 错误：无法推送某些 ref 至 'https://github.com/USERNAME/REPOSITORY.git'
> 为防止丢失历史记录，非快进更新已被拒绝
> 再次推送前合并远程更改（例如： ‘git pull’）。 [rejected]        main -> main (non-fast-forward)
> error: failed to push some refs to 'https://github.com/USERNAME/REPOSITORY.git'
> To prevent you from losing history, non-fast-forward updates were rejected
> Merge the remote changes (e.g. 'git pull') before pushing again.  请参阅
> “git 推送帮助”部分的“快进说明”以了解详细信息。
```

通过获取和合并远程分支上所做更改与本地所做更改，您可以解决此问题：

```bazaar
$ git fetch origin
# Fetches updates made to an online repository
$ git merge origin YOUR_BRANCH_NAME
# Merges updates made online with your local work
```

或者，可以只是使用 git pull 一次性执行两个命令：

```bazaar
$ git pull origin YOUR_BRANCH_NAME
# Grabs online updates and merges them with your local work
```

原文：https://docs.github.com/cn/free-pro-team@latest/github/using-git/dealing-with-non-fast-forward-errors
