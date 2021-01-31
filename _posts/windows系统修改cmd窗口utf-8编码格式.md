---
title: windows系统修改cmd窗口utf-8编码格式
url: windows系统修改cmd窗口utf-8编码格式
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-6746'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T20:03:06+08:00'

---

永久修改：win键+R，输入regedit，确定。

windows系统修改cmd窗口utf-8编码格式
按顺序找到

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor

windows系统修改cmd窗口utf-8编码格式
点击右键-新建，选择“字符串值”。

windows系统修改cmd窗口utf-8编码格式
命名为“autorun”, 点击右击修改，数值数据填写“chcp 65001”，确定。

windows系统修改cmd窗口utf-8编码格式
这时候打开cmd命令窗口就会看到，和之前临时修改的窗口一样，编码已经修改成UTF-8了，而且每次打开cmd都是UTF-8编码。
