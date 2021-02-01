---
title: IntelliJ IDEA 统一设置编码为utf-8编码
siteurl: intellij-idea-统一设置编码为utf-8编码
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-intellij-idea-统一设置编码为utf-8编码
date: '2021-01-08T17:04:15+08:00'
date updated: '2021-01-08T17:07:18+08:00'

---

> Win any way as long as you can get away with it. Nice guys finish last.
> — <cite>Leo Durocher</cite>

**问题一：**

File->Settings->Editor->File Encodings

![img](images/20180608220601158.jpg)

**问题二：**

File->Other Settings->Default Settings ->Editor->File Encodings

![img](images/20180608212648780.jpg)

**问题三：**

将项目中的**.idea文件夹**中的****encodings.xml****文件中的编码格式改为uft-8

**问题四：**

File->Settings->Build,Execution,Deployment -> Compiler -> Java Compiler

设置 **Additional command line parameters**选项为 **-encoding utf-8**

![img](images/20180608215004536.jpg)

**问题五：**

1.jpg)打开Run/Debug Configuration,选择你的tomcat

![img](images/20180608220138855.jpg)

2.jpg) 然后在 Server > VM options 设置为 -Dfile.encoding=UTF-8 ，重启tomcat

![img](images/20180608220247543.jpg)

**问题六：**

清空浏览器缓存再试一次。
