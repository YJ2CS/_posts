---
title: pychram_manager_repo_镜像源
siteurl: pychram-manager-repo-镜像源
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - IDE
tags:
  - IDE
no-photos: >-
  https://random.52ecy.cn/randbg.php?size=1&rid-4e73
date: 2020-11-10 22:16:00
---

# 关于国内镜像源
关于国内镜像源的问题: 因为一些众所周知的原因，我们需要把软件源从官方替换为国内镜像网站

[官方文档](https://www.jetbrains.com/help/pycharm/project-interpreter.html#packages)

[manger repo](https://www.jetbrains.com/help/pycharm/available-packages.html)

![](68531d3e.png)

由于国外的镜像源安装Python速度较慢，选择国内的镜像速度较快，这篇文章如要讲述如何设置国内镜像源。
常用镜像源：
```
清华：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

```

方法一：
在安装包的时候执行命令（以安装Numpy为例）：
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple numpy
```

方法二：
```
Step1:
打开Settings…

Step2:
搜索Project Interpreter

Step3:
双击上一步任意一个Package文件名，弹出如下界面（Available Packages），选择Manage Repositories：

Step 4:
选择右上角的加号，添加镜像源：
```

