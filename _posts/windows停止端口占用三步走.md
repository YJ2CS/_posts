---
title: Windows停止端口占用三步走
url: windows停止端口占用三步走
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 开发环境
tags:
  - 环境配置
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-a942'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T11:55:21+08:00'

---

# Windows停止端口占用三步走

输入命令：netstat -ano，列出所有端口的情况。在列表中我们观察被占用的端口，比如是1224，首先找到它；

查看被占用端口对应的PID，输入命令：netstat -aon|findstr “8081”，回车，记下最后一位数字，即PID,这里是9088；

输入tasklist|findstr “9088”，回车，查看是哪个进程或者程序占用了8081端口，结果是：node.exe

停止端口占用:taskkill /pid 9088 /f
