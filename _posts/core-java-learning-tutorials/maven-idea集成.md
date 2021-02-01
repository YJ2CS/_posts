---
title: Maven:IDEA集成
siteurl: maven-idea集成
alias:
  - 5-1-7-idea集成/index.html
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - learn-java
tags:
  - java
  - maven
  - java进阶
  - 第5部分-1周
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-d480
date: 2020/11/22
date updated: '2021-01-28T11:20:06+08:00'

---

> 目录[[java目录]]
> 下一节: [[maven-nexus私服]]

[IDEA的官方文档](https://www.jetbrains.com/help/idea/2020.2/maven.html)
  Maven项目对象模型(POM)，是一个项目管理工具可以通过一小段描述信息来管理项目的构建，报告和文档的软件。那我们想要在IDEA中使用Maven得进行一些配置,那接下来\
我们具体看一下是如何配置使用的?

IntelliJ IDEA 的一些特性列出如下：

- 可以通过 IntelliJ IDEA 来运行 Maven 目标。
- 可以在 IntelliJ IDEA 自己的终端里查看 Maven 命令的输出结果。
- 可以在 IDE 里更新 Maven 的依赖关系。
- 可以在 IntelliJ IDEA 中启动 Maven 的构建。
- IntelliJ IDEA 基于 Maven 的 pom.xml 来实现自动化管理依赖关系。
- IntelliJ IDEA 可以通过自己的工作区解决 Maven 的依赖问题，而无需安装到本地的 Maven 仓库，虽然需要依赖的项目在同一个工作区。
- IntelliJ IDEA 可以自动从远程 Maven 仓库上下载需要的依赖和源码。
- IntelliJ IDEA 提供了创建 Maven 项目，pom.xml 文件的向导。

## 下载Maven

### 使用IDEA捆绑的Maven

IntelliJ IDEA 已经内建了对 Maven 的支持。

下面我们配置一下Maven的本地仓库路径,首先找到解压Maven的目录

找到`C:\Users\YourUserName\.m2\settings.xml`这个配置文件打开

![](https://img-blog.csdn.net/20180104102013323)

打开settings.xml 配置文件 选一个本地的目录作为Maven本地仓库将配置好

```xml
<localRepository>D:\my_maven_local_repository</localRepository>
```

![](https://img-blog.csdn.net/20180104102055790)

至此我们的Maven工具就算安装并配置好了.

### Maven官网下载

一、 首先我们得去Maven的官网去下载Maven 网址: <http://maven.apache.org/download.cgi>

二、 进入Maven官网后如下图点击下载

![](https://img-blog.csdn.net/20180104101509936)

三、解压此Maven的压缩包,注意不要解压到中文路径下,切记!!! 如图

![](https://img-blog.csdn.net/20180104101714564)

四、 解压完后,Maven这个工具就算安装好了,但是我们还需要配置一下Maven的环境变量

五、此电脑——右键——属性——高级系统设置——环境变量——系统变量——新建——变量名和变量值

![](https://img-blog.csdn.net/20180104101835212)

六、 在系统变量PATH中引入你配置的变量名:  MAVAEN_HOME%MAVEN_HOME%\bin;

![](https://img-blog.csdn.net/20180104101918795)

七、下面我们配置一下Maven的本地仓库路径,首先找到解压Maven的目录

找到conf-——settings.xml这个配置文件打开

![](https://img-blog.csdn.net/20180104102013323)

打开settings.xml 配置文件 选一个本地的目录作为Maven本地仓库将配置好

```xml
<localRepository>D:\my_maven_local_repository</localRepository>
```

![](https://img-blog.csdn.net/20180104102055790)

八、 至此我们的Maven工具就算安装并配置好了.

接下来是与idea的集成

##     1、在idea中对maven进行集成

首先需要在idea中对maven进行集成，目录为File》Setting》Build、Execution、Deployment》Build Tools》maven，若打开idea之前已经安装了maven，则idea会自动发现maven并进行关联，如下图：

![](https://img-blog.csdn.net/20180623130645301?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

并且需要注意maven的选相关配置：

Maven home directory：maven的地址

setting.xml：若项目中使用的maven私服则需要进行配置

maven respository：经常需要关注的maven仓库地址    

##     2、import配置

maven下的import使用中经常需要关注的地方，目录File》Setting》Build、Execution、Deployment》Build Tools》maven》import，如下图：

![](https://img-blog.csdn.net/20180623131817759?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

import Maven project automatically：自动监控pom.xml的改动，并且进行导入maven依赖

Dependency Type：依赖类型

Automatically down（Sources、Documentation）：是否自动下载源码和java doc文档（与eclipse中一致），我一般会进行勾选，这样查看源码非常方便

vm和jdk设置：需要时候可以进行设置

**二、idea中maven的使用**

在使用maven项目的时候，使用最多的是Maven Project视图，若不进行显示，则可以在View》Tool Buttons 中进行勾选，如下图：

![](https://img-blog.csdn.net/20180623135426138?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 1、maven操作

###     1)、Reimport All Maven Projects

![](https://img-blog.csdn.net/20180623135746922?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)根据pom文件重新加载（导入）文件

###     2)、Generate Sources and Update Folders For All Project

![](https://img-blog.csdn.net/2018062314034726?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)让源代码重新进行编译

###     3)、Download Resource and/or Document

![](https://img-blog.csdn.net/20180623140445845?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)下载源码和文档

###     4)、Add Maven Projects

![](https://img-blog.csdn.net/20180623140546163?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)添加一个maven项目

###     5)、Run maven Build

![](https://img-blog.csdn.net/20180623140643182?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

执行选中的命令,如下面Lifeclcle中的命令

![](https://img-blog.csdn.net/20180623141928717?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###     6)、Execute Maven Goal

![](https://img-blog.csdn.net/2018062314183839?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)执行mvn命令或自定义的命令，如：

![](https://img-blog.csdn.net/20180623141918283?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###     7)、Toggle Offline Mode

![](https://img-blog.csdn.net/20180623142332945?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)关闭和远程仓库的链接，即版本管理工具不能提交到远程

###     8)、Toggle 'Skip Tests' Mode

![](https://img-blog.csdn.net/20180623142834864?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)跳过maven生命周期的测试环节

###     10)、Show Dependencies（Ctrl+Alt+Shift+U）

 ![](https://img-blog.csdn.net/20180623142753886?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   展示当前选中的maven依赖，比使用生成依赖树方便很多，并且可以直接在图形化树上进行排除依赖操作，如下：

![](https://img-blog.csdn.net/20180623143342916?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 11)、Collapse All（Ctrl+NumPad -）

![](https://img-blog.csdn.net/20180623143433249?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)收起下面展开的树形

### 12)、Maven Setting

![](https://img-blog.csdn.net/20180623143531368?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0X2xpaG9uZ21pbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  跳转到maven的Setting页面

## 2、快速命令

###     1)、LifeCycle

快速的maven生命常用命令，如：clean、install、deploy等，

###     2)、Plugins

项目中依赖的maven插件，我非常喜欢使用tomcat（或者tomcat7）的maven插件，svn tomcat:run 命令启动项目，将在IntelliJ IDEA集成tomcat中进行讲解。

###     3)、Dependencies

- [转载原文地址](https://blog.csdn.net/it_lihongmin/article/details/80782740)
