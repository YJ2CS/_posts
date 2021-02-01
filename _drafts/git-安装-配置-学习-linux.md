---
title: linux下安装、配置、学习使用Git
siteurl: git-安装-配置-学习-linux
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-git-安装-配置-学习-linux'
date: '2021-01-24T16:58:20+08:00'
date updated: '2021-01-24T16:58:20+08:00'

---

> All courses of action are risky, so prudence is not in avoiding danger (it's impossible), but calculating risk and acting decisively. Make mistakes of ambition and not mistakes of sloth. Develop the strength to do bold things, not the strength to suffer.
> &mdash; <cite>Niccolo Machiavelli</cite>


在 WSL 下用 vscode 打开 git 项目, `git status` 会显示文件被修改(实际上没有), 这可能是因为 Linux 和 Windows 的不同换行符造成的.
在 WSL 下设置 `git config --global core.autocrlf true` 来忽略它即可解决.

```shell
git config --global core.autocrlf true
```