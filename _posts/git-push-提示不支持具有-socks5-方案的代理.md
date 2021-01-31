---
title: Git Push 提示不支持具有 Socks5 方案的代理
url: git-push-提示不支持具有-socks5-方案的代理
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-5a63
date: 2020-11-10 22:16:00
---


## 场景
使用 Git Push 提交代码到远程服务器时提示了一个错误

fatal: NotSupportedException encountered.

   ServicePointManager 不支持具有 socks5 方案的代理。
   
## 问题

然而之后还是正常提交成功了，实际上问题是：

配置了本地的 socks5 的代理（Shadowsocks 之类的代理软件）

配置了远程服务器 Git 服务端的 SSH

本地提交代码到远程服务器时使用的是 http/https 协议

这三者只要有一个不满足就不会出现这个错误了

## 解决方案

1.取消远程的 SSH

在下面的页面中删除你的 SSH Keys 即可

[GitHub](https://github.com/settings/keys)

[Bitbucket](https://bitbucket.org/account/user/your_username/ssh-keys/)

2.提交内容到远程 Git 服务器时选择 SSH 协议

设置远程仓库为 SSH 协议，例如 GitHub 的 SSH 链接就是 <git@github.com:rxliuli/rxliuli.github.io.git>

好了，关于 Git 提示错误 Git Push 提示不支持具有 Socks5 方案的代理 就到这里啦