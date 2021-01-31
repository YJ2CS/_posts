---
title: java11关于Windows平台使用idea+openjfx运行JavaFX程序的踩坑
url: java11关于windows平台使用idea-openjfx运行javafx程序的踩坑
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-154d'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T11:47:35+08:00'

---

中文互联网上没有一个靠谱的答案，最后在Stack Overflow上找到了问题实际的原因和解决方法

之后又找到了两位大神，解析了其中的技术原理

先贴出大神的文章:

<https://taylorial.com/cs1021/Install.htm>

<https://my.oschina.net/tridays/blog/2222909>

## 准备

本文使用的javafx是openjfx,
给出openjfx的官方文档，大家可以收藏，也可以直接查阅，很有用处

[openjfx](https://openjfx.io/openjfx-docs/#install-javafx)

## 解答误区

1.  两个IDEA中关于javafx的插件

![](91772390.png)

其中第一个是在idea中新建一个javafx项目所必须的，

第二个是使用Oracle公司的javafx的必备插件，

鉴于本文使用的是openjfx，它们和本文的解决方案都没有关系

2.  [openjfx的官方文档](https://openjfx.io/openjfx-docs/#install-java) 中有说明

![](ff803b0a.png)

其中%PATH_TO_FX%一项实际上是指代一个确定的、实际的**绝对路径**，既上图中描述的，

假如你的openjfx安装路径是"C:\Program Files\Java\javafx-sdk-11.0.1\lib"

将图中引号部分的内容给替换为"C:\Program Files\Java\javafx-sdk-11.0.1\lib"
就行

3.  中文互联网上信息更新明显慢外网两年，建议大家抛弃百度，使用谷歌，然后把你的问题翻译成英文搜索答案，找到正确答案后再翻译回来阅读。

## 解决方法一

1.  在library中添加openjfx目录下的lib文件夹，并将其指定应用在你需要import javafx包的模块中

File -> Project Structure -> Libraries

add the JavaFX11/lib/

![](a978aa4b.png)
![](7d282511.png)

2.

IntelliJ->File->Settings->Appearance & Behavior->Path Variables

![](161f51f7.png)

新增一个PATH_TO_FX变量，路径指向openjfx/lib文件夹

-   openjfx/lib是你实际安装的openjfx路径下lib文件夹，比如我的是"C:\Program Files\Java\javafx-sdk-11.0.1\lib"

例如：
![](f4b5f99f.png)

然后点击Run -> Edit Configurations...，,

![](1ae96ef2.png)

并在 VM 选项(VM options)中添加路径：

```java
--module-path ${PATH_TO_FX} --add-modules=javafx.controls,javafx.fxml
```

$PATH_TO_FX$或者${PATH_TO_FX}两种写法都可以

![](09c07a51.png)

注意一定是要修改你实际上需要运行的那个类(即含有import javafx语句的那个类)
![](eb6520cd.png)

当然你也可以写成绝对路径，但我并不推荐。
步骤如下：

在 VM 选项(VM options)中添加路径：

    --module-path "C:\Program Files\Java\javafx-sdk-11.0.1\lib" --add-modules=javafx.controls,javafx.fxml

![](68c3a8ca.png)

其中"C:\Program Files\Java\javafx-sdk-11.0.1\lib"要换成你的javafx的实际路径

注意一定是要修改你实际上需要运行的那个类(即含有import javafx语句的那个类)
![](eb6520cd.png)

## 解决方案二

这里先学习一个知识点：

模块化 Java 程序与非模块化 Java 程序的启动方式有所不同。

# 非模块化

    java [options] mainclass [args...]

# 模块化

```
java [options] [--module-path modulepath] --module module[/mainclass] [args...]

```

提供了 module-info.java 的话，IDEA 发现这是模块化的 Java 程序。以上例为例，启动命令是：

```
java ${OPTIONS} -m ${METHOD_PATH} -m sample/sample.JFXMain

```

否则，IDEA 会认为这是非模块化 Java 程序，启动命令是：

```
java ${OPTIONS} -classpath ${CLASS_PATH} sample.JFXMain

```

具体到本文，一个常见错误是，启动报错：缺少 JavaFX 运行时组件, 需要使用该组件来运行此应用程序

我们在 JDK 11 的 sun.launcher.LauncherHelper 发现：如果 JFXMain 继承自 javafx.application.Application，同时程序从 JFXMain.main() 启动，LauncherHelper 会检查是否存在模块 javafx.graphics 的声明：

```java
package sun.launcher;

public final class LauncherHelper {

    static final class FXHelper {

        private static void setFXLaunchParameters(String what, int mode) {
            ...
            Optional<Module> om = ModuleLayer.boot().findModule(JAVAFX_GRAPHICS_MODULE_NAME);
            if (!om.isPresent()) {
                abort(null, "java.launcher.cls.error5");
            }
            ...
        }
    }
}
```

显然，如果不以模块化 Java 程序的方式启动，没有模块信息。错误码 java.launcher.cls.error5 即为 “错误: 缺少 JavaFX 运行时组件, 需要使用该组件来运行此应用程序。”

那么就引出了解决方案二

### 解决方法

我们还有其他办法来绕开 LauncherHelper 的检查，能够以非模块化 Java 程序的方式运行程序。思路是：使程序的入口 main() 不继承自 javafx.application.Application。

而我们又知道, maven 的 main() 显然满足该要求。这用到了 exec-maven-plugin，这个插件是默认包含的，我们可以直接使用它的 property exec.mainClass。

我们可以使用 maven 来运行程序.

修改 pom.xml：

```xml
<properties>
    ...
    <exec.mainClass>sample.JFXMain</exec.mainClass>
    ...
</properties>
```

运行命令如下：

```bazaar
mvn clean compile exec:java
```

## 方案三

我们也可以单独创建一个启动类：

```java
package sample;

import javafx.application.Application;

public class AppMain {

    public static void main(String[] args) {
        Application.launch(JFXMain.class, args);
    }
}
```

从这个类启动 Java 程序，效果与方案二相同。

## 另外一个常见报错

编译报错：Error: (4, 1) java: -source 8 中不支持 模块

根据上文所述的方法，检查并修改 Project bytecode version。

## 延申——使用maven打包openjfx11项目

给出文章链接：https://my.oschina.net/u/4231975/blog/4295294
