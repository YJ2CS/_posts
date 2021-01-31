---
title: Maven：包管理简介
url: maven-包管理简介
alias:
  - 5-1-maven与包管理/index.html
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-d57a
date: 2020-11-10T22:16:00.000Z
date updated: '2021-01-28T11:24:06+08:00'

---

> 目录：[[java目录]]
> 下一节: [[maven-pom文件详解]]

## Maven安装与集成

[安装与集成](https://blog.csdn.net/Sugar_map/article/details/80262080)

[idea官方文档](https://www.jetbrains.com/help/idea/2020.2/maven-support.html#change_jdk)

[Maven官方文档](https://maven.apache.org/guides/index.html)

## 什么是Java 包

为了更好地组织类，Java 提供了包机制，用于区别类名的命名空间。

### 包的作用

- 把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用。
- 如同文件夹一样，包也采用了树形目录的存储方式。同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别。因此，包可以避免名字冲突。
- 包也限定了访问权限，拥有包访问权限的类才能访问某个包中的类。

Java 使用包（package）这种机制是为了防止命名冲突，访问控制，提供搜索和定位类（class）、接口、枚举（enumerations）和注释（annotation）等。

## JVM的工作原理

JVM的工作被设计的很简单：

- 用import引入包的操作标记了包的地址，而当JVM读到包名时会从包的地址找到包本身，然后执行包中的代码，JVM通过`classpath`参数来获取包的路径。
- Java代码的执行是从将人所编写的代码编译为JVM所执行的机器码开始的。JVM会执行一个类的字节码，在执行这个类的字节码的时候，若碰到了新的类，则加载它，如此往复。

由于一个包有可能又依赖于其他很多个包，因此一个项目下来，可能classpath下的依赖路径会变得很长，早年人们是需要通过手写这些`classpath`路径来让JVM读懂读取jar包（一堆类的集合）的路径的，后来随着技术的发展Maven应运而生。

## Maven

Maven是一个项目管理工具，它包含了一个项目对象模型 (`Project Object Model`)，一组标准集合，一个项目生命周期(`Project Lifecycle`)，
一个依赖管理系统(`Dependency Management System`)，和用来运行定义在生命周期阶段(`phase`)中插件(`plugin`)目标(`goal`)的逻辑。
当你使用Maven的时候，你用一个明确定义的项目对象模型来描述你的项目，然后Maven可以应用横切的逻辑，这些逻辑来自一组共享的（或者自定义的）插件。

Maven 有一个生命周期，当你运行 mvn install 的时候被调用。这条命令告诉 Maven 执行一系列的有序的步骤，直到到达你指定的生命周期。
遍历生命周期旅途中的一个影响就是，Maven 运行了许多默认的插件目标，这些目标完成了像编译和创建一个 JAR 文件这样的工作。 此外，Maven能够很方便的帮你管理项目报告，生成站点，管理JAR文件，等等。

### 约定配置

Maven 提倡使用一个共同的标准目录结构，Maven使用约定优于配置的原则，大家尽可能的遵守这样的目录结构。如下所示：

![](https://user-gold-cdn.xitu.io/2020/7/14/1734d9f714ec1459?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 传递依赖 与 排除依赖

- 传递依赖：如果我们的项目引用了一个Jar包，而该Jar包又引用了其他Jar包，那么在默认情况下项目编译时，Maven会把直接引用和简洁引用的Jar包都下载到本地。
- 排除依赖：如果我们只想下载直接引用的Jar包，那么需要在pom.xml中做如下配置：(将需要排除的Jar包的坐标写在中)

```xml
<exclusions>
    <exclusion>
        <groupId>cn.missbe.web.search</groupId>
        <artifactId>resource-search</artifactId>
        <packaging>pom</packaging>
        <version>1.0-SNAPSHOT</version>
    </exclusion>
</exclusions>
复制代码
```

### 依赖范围scope

在项目发布过程中，帮助决定哪些构件被包括进来。

- compile ：默认范围，用于编译
- provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath
- runtime: 在执行时需要使用
- test: 用于test任务时使用
- system: 需要外在提供相应的元素。通过systemPath来取得
- systemPath: 仅用于范围为system。提供相应的路径
- optional: 当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用

### 依赖冲突

若项目中多个Jar同时引用了相同的Jar时，会产生依赖冲突，但Maven采用了两种避免冲突的策略，因此在Maven中是不存在依赖冲突的。

#### 短路优先

本项目——>A.jar——>B.jar——>X.jar

本项目——>C.jar——>X.jar

若本项目引用了A.jar，A.jar又引用了B.jar，B.jar又引用了X.jar，并且C.jar也引用了X.jar。 在此时，Maven只会引用引用路径最短的Jar。

#### 声明优先

若引用路径长度相同时，在pom.xml中谁先被声明，就使用谁。

### Maven的中央仓库

Maven 中央仓库是由 Maven 社区提供的仓库，其中包含了大量常用的库。 中央仓库包含了绝大多数流行的开源Java构件，以及源码、作者信息、SCM、信息、许可证信息等。一般来说，简单的Java项目依赖的构件都可以在这里下载到。 中央仓库的关键概念：

- 这个仓库由 Maven 社区管理。
- 不需要配置。
- 需要通过网络才能访问。

要浏览中央仓库的内容，maven社区提供了一个URL：[search.maven.org/#browse](http://search.maven.org/#browse)。使用这个仓库，开发人员可以搜索所有可以获取的代码库。

#### Maven的本地仓库

Maven 的本地仓库，在安装 Maven 后并不会创建，它是在第一次执行maven命令的时候才被创建。 运行 Maven 的时候，Maven所需要的任何构件都是直接从本地仓库获取的。如果本地仓库没有，它会首先尝试从远程仓库下载构件至本地仓库，然后再使用本地仓库的构件。

默认情况下，不管Linux还是 Windows，每个用户在自己的用户目录下都有一个路径名为 .m2/respository/ 的仓库目录。 Maven 本地仓库默认被创建在`%USER_HOME%` 目录下。
要修改默认位置，在 `%M2_HOME%\conf` 目录中的 Maven 的 `settings.xml` 文件中定义另一个路径。

本文转载并修改自以下作者

- [韓陳昊](https://juejin.cn/post/6850418117948997645)
