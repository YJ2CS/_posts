---
title: 记录wsl2配置和使用
siteurl: 记录wsl2配置和使用
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 开发环境
tags:
  - 环境配置
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-93fa
date: 2020-11-16T17:54:00.000Z
date updated: '2021-01-25T21:58:34+08:00'

---

wsl2是一个完整的linux系统环境，支持docker等服务，本文讲讲wsl2里环境的配置。

## WSL2 安装

在最新版Windows2004内核下，wsl2的安装包只有1.05MB左右，貌似已内置了。查看Windows内核方法，win + R键，输入winver回车就可以看到。本站为了尝试新版wsl2的CUDA显卡计算特性，特意升级到了预览版。

官方文档. <https://docs.microsoft.com/en-us/windows/wsl/wsl2-install>

如何开启 wsl [Windows Subsystem for Linux Installation Guide for Windows 10](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/en-us/windows/wsl/install-win10)

笔者熟悉 Ubuntu 的使用, WSL 也对 Ubuntu 有很好的支持. 全文皆以 Ubuntu18.04 为例说明.

为了简便, 有时使用 `WSL` 指代 `WSL2` .

安装wsl很简单，只需到微软商店搜wsl或linux或ubuntu之类的关键词，就可以找到。默认选择ubuntu20.04LST版本。

安装完毕后，就可以在Windows Termial输入wsl -l -v查看当前的子系统是否为wsl2

## Win10重置/注销/备份/恢复Linux子系统

这部分内容严格来说只会在你的wsl出现错误情况下用到
[[win10重置注销备份恢复wsl2]]

## WSL 修改默认登陆用户

以下方法适用于 Fall Creators Update 及更高版本:

假如我们在 WSL 版本的 Ubuntu 18.04 LTS 中忘记密码的时候，需要切换到 root 用户修改密码。

使用管理员打开 PowerShell，输入：

```shell
ubuntu1804.exe config --default-user root
```

即可修改成默认使用用户名为 root 的用登录。

同理，在ubuntu2004中可以这样输入

```shell
ubuntu2004.exe config --default-user root
```

您也可以使用以下命令

```shell
wsl config --default-user root
```

## WSL2设置网络代理

[[类linux环境设置网络代理]]

## WSL2下配置Git

[[git-安装-配置-学习-linux|WSL2下配置Git]]

## WSL2下的Docker 配置

[[docker-wsl2|WSL2下的Docker 配置]]

## wsl2下的宝塔安装问题

[[linux环境下的宝塔安装配置]]

## 附转载链接

> [Windows 10 WSL2 安装使用及Docker安装](https://www.jiebaiyou.com/2020/06/05/Windows-10-WSL2-%E5%AE%89%E8%A3%85%E4%BD%BF%E7%94%A8%E5%8F%8ADocker%E5%AE%89%E8%A3%85/)
>
> <https://blog.nediiii.com/wsl2-note/>
>
> <https://baiyue.one/archives/1140.html>
