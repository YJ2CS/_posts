---
title: win10重置注销备份恢复wsl2
url: win10重置注销备份恢复wsl2
alias:
  - win10重置注销wsl2/index.html
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-win10重装注销wsl2
date: '2021-01-24T16:52:17+08:00'
date updated: '2021-01-25T21:58:30+08:00'

---

> All courses of action are risky, so prudence is not in avoiding danger (it's impossible), but calculating risk and acting decisively. Make mistakes of ambition and not mistakes of sloth. Develop the strength to do bold things, not the strength to suffer.
> — <cite>Niccolo Machiavelli</cite>

如果Windows10启用“适用于Linux的Windows子系统(WSL)”安装的某个Linux发行版子系统（例如Ubuntu）出现了问题，或者我们想进行重新配置，那么我们可以重置或注销该Linux子系统，这样再次启动该Linux子系统时，Win10系统就会重新安装一个全新的Linux子系统，包括用户名和密码等都需要我们全新设置。下面我就来分享一下重置和注销Linux子系统的方法步骤：

## 重置Linux子系统

在《Windows设置》中即可直观地进行操作。进入“Windows设置 - 应用 - 应用和功能”设置界面，在窗口右侧会显示Win10系统所安装的所有应用程序，其中就有你通过Microsoft Store安装的Linux子系统（例如Ubuntu）。如图：

“Windows设置 - 应用 - 应用和功能”显示有通过Microsoft Store安装的Linux子系统

选中Ubuntu，点击“高级选项”转到如图所示的设置界面：

Ubuntu“高级选项”设置界面的“重置”按钮

顶部“规格”显示了“发布者、版本号、应用程序和数据所占空间”等信息。下面则依次显示“终止、修复、重置、卸载”等按钮。

“修复”和“重置”均位于“重置”区域下，二者都能解决Ubuntu无法正常运行的问题，区别在于：“修复”不会使应用数据丢失，而“重置”则会抹去所有应用数据和设置，因为它会重装安装Ubuntu应用。所以如果Ubuntu应用出现问题，请先尝试“修复”，如果无效再尝试“重置”。

点击“重置”按钮会弹出提示“这将永久删除此设备上的应用数据，包括你的首选项和登录详细信息”。如图：

这将永久删除此设备上的应用数据，包括你的首选项和登录详细信息

点击“重置”，系统就会进行重置Ubuntu的操作，待完成后会在“重置”按钮旁边显示一个对号。

然后你再在Win10开始菜单中运行Ubuntu，就会显示正在安装。

安装完成后就会让你设置用户名和密码，就像当初全新安装Ubuntu时一样。

## 注销Linux子系统

注销Linux子系统和重置的效果类似，也会删除所有的应用数据和设置，但和“卸载”不一样，注销之后，它依然会显示在Win10开始菜单中，当你点击它时，就会和前面“重置”后一样进行全新安装和设置。下面我分享一下注销Linux子系统的方法步骤：

首先，我们需要获知当前Win10系统中安装了哪些Linux发行版子系统。以管理员身份运行命令提示符，输入并回车运行以下命令：

```shell
wsl.exe --list --all
```

你可以看到当前只安装了Ubuntu这一个子系统。那就只有注销它了。

其实我前几天在分享Win10导入Linux子系统教程时已经简单介绍了注销方法，只需运行命令：

```shell
wsl.exe --unregister Ubuntu
```

就会提示“正在注销”，等待注销完成即可。

注销之后你再运行前面的命令查询当前Win10系统中安装的Linux子系统，就看不到Ubuntu了。但是在Win10开始菜单中却依然能够看到它，并且点击运行它，就会和前面的“重置”之后一样显示“正在安装”。然后你就可以进行全新设置了。

## 查看帮助

```shell
wsl --help # 查看帮助
```

## 备份

```shell
# wsl --export <Distro> <FileName>
# 1. 查看Distro: wsl --list
# 例如: wsl --export Ubuntu-18.04 D:\u.tar
```

## 恢复

```shell
# wsl --import <Distro> <InstallLocation> <FileName> [Options]
```
