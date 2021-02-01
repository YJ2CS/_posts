---
title: docker wsl2 hyperv proxifier 踩坑
siteurl: docker-wsl2-hyper-v-proxifier-踩坑
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-fd2a'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T19:59:01+08:00'

---

安装docker需要wsl2的环境和hyper-v的环境
先简述安装过程：

全部贴官方文档

## 安装wsl2

<https://docs.microsoft.com/zh-cn/windows/wsl/install-win10>

注意：

若要将分发版设置为受某一 WSL 版本支持，请运行：

```Shell

wsl --set-version <distribution name> <versionNumber>

* <distribution name>就是distro,也就是Linux发行版，对应我的发行版就是Ubuntu。

<versionNumber>要填2 因为我们需要安装wsl2

```

## 启用hyper-v

<https://docs.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v>

## 配置docker

从install一节开始看

<https://docs.docker.com/docker-for-windows/wsl/>

## 坑

### hyper-v和VM不能共存

```shell
# 当使用Docker时，需要开启Hyper-v，设置
bcdedit /set hypervisorlaunchtype auto

# 当使用虚拟机是，需要关闭Hyper-v，设置
bcdedit /set hypervisorlaunchtype off

```

### Proxifier和wsl2不能共存

关于proxifier的解决方法，下面贴一段github上的原文和翻译

简述就是

```text
Works for me, steps:

将NoLsp.exe放到D盘根目录

Run powershell as Administrator.

Run command cd d:/

Run command .\NoLsp.exe c:\windows\system32\wsl.exe

Then you well see return: Success!
```

```text
Proxifier的开发人员告诉了我原因并给出了解决方案。谢谢你的信息。
我们转载了这一期。显然地，wsl.exe文件如果Winsock LSP DLL加载到其进程中，则显示此错误。
最简单的解决方案是使用WSCSetApplicationCategory WinAPI调用wsl.exe文件为了防止这种情况。
在引擎盖下，调用为wsl.exe文件在HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinSock2\Parameters\AppId_Catalog中，
这会告诉Windows不要将LSP DLL加载到wsl.exe文件过程。
我们有一个工具可以进行此调用：www.proxifier.com/tmp/Test20200228/NoLsp.exe
请以管理员身份运行，完整路径为wsl.exe文件作为参数：
NoLsp.exe c:\windows\system32\wsl.exe
这就解决了我的问题。请让我知道它是如何为你工作的。

```

```text
Developers of Proxifier told me the reason and gave me a solution.

Thanks for the info.
We have reproduced this issue.
Apparently, wsl.exe displays this error if Winsock LSP DLL gets loaded into its process.
The easiest solution is to use WSCSetApplicationCategory WinAPI call for wsl.exe to prevent this.
Under the hood the call creates an entry for wsl.exe at HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinSock2\Parameters\AppId_Catalog
This tells Windows not to load LSP DLLs into wsl.exe process.
We have a tool that can make this call:
www.proxifier.com/tmp/Test20200228/NoLsp.exe
Please just run as admin with the full path to wsl.exe as the parameter:
NoLsp.exe c:\windows\system32\wsl.exe
This has fixed the problem in my case.
Please let me know how it works for you.

```
