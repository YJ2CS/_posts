---
title: >-
  This application failed to start because no Qt platform plugin could be
  initialized
siteurl: >-
  this-application-failed-to-start-because-no-qt-platform-plugin-could-be-initialized
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-82b1'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-26T19:53:49+08:00'

---

[This application failed to start because no Qt platform plugin could be initialized](https://www.cnblogs.com/focksor/p/13166970.html)

This application failed to start because no Qt platform plugin could be initialized

## 错误提示[#](https://www.cnblogs.com/focksor/p/13166970.html#2077802124)

在打开PyQt5-tools中的Designer.exe时，提示如下错误：

[![image-20200619231239409](https://gitee.com/focksor/giteePagesImgs/raw/master/image-20200619231239409.png)

> designer
>
> This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application

may fix this problem.

## 解决方法[#](https://www.cnblogs.com/focksor/p/13166970.html#3178058157)

定位到`Python38\Lib\site-packages\PyQt5\Qt\plugins`，这个位置取决于你安装Python的位置，我的电脑上该位置的完整路劲是：`C:\Users\focksor\AppData\Local\Programs\Python\Python38\Lib\site-packages\PyQt5\Qt\plugins`。

将该目录下的`platforms`文件夹复制到`Python38\Lib\site-packages\pyqt5_tools\Qt\bin`，并替换处于该目录下的`platforms`内的文件。

再次打开`designer.exe`，此时问题应该已解决。

## 参考[#](https://www.cnblogs.com/focksor/p/13166970.html#3116096095)

[PyQt5 Designer is not working: This application failed to start because no Qt platform plugin could be initialized](https://stackoverflow.com/questions/61324972/pyqt5-designer-is-not-working-this-application-failed-to-start-because-no-qt-pl)

## 提示[#](https://www.cnblogs.com/focksor/p/13166970.html#3574183339)

如果你同样遇到该问题并通过此贴解决了问题，你可以留下你的评论来提示后来人该方法是可行的。

作者： focksor

出处：<https://www.cnblogs.com/focksor/p/13166970.html>

版权：本文采用「[署名-非商业性使用-相同方式共享 4.0 国际](https://creativecommons.org/licenses/by-nc-sa/4.0/)」知识共享许可协议进行许可。
