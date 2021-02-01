---
title: Maven：内置命令
siteurl: maven-内置命令
alias:
  - 5-1-3-maven内置命令/index.html
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-769e
date: 2020/11/22
date updated: '2021-01-28T11:19:03+08:00'

---

> 目录：[[java目录]]
> 下一节: [[maven-生命周期]]

**maven 命令的格式为`mvn [plugin-name]:[goal-name]`，可以接受的参数如下。**

`-D` 指定参数，如 -Dmaven.test.skip=true 跳过单元测试；

`-P` 指定 Profile 配置，可以用于区分环境；

`-e` 显示maven运行出错的信息；

`-o` 离线执行命令,即不去远程仓库更新包；

`-X` 显示maven允许的debug信息；

`-U` 强制去远程更新snapshot的插件或依赖，默认每天只更新一次。

## 常用maven命令速览

`mvn -v` 查看maven版本

`mvn compile` 编译

`mvn test` 测试

`mvn package` 打包

`mvn clean` 删除target

`mvn install` 安装jar包到本地仓库中

构建项目的两种方式：

`mvn archetype:generate` 按照提示构建maven项目，下面maven依赖。首次运行时，mvn会从远程“中央仓库”下载必要的文件到“本地仓库”

`mvn archetype:generate`

`-DgroupId=`组织名，公司网站反写+项目名/Users/ken/Desktop/maven学习.txt

`-DartifactId=`项目名-模块名

`-Dversion=`版本号

`-Dpackage=`代码所存储的包名

`-DarchetypeArtifactId=`指定ArchetypeId

`maven-archetype-quickstart`，创建一个Java Project；

`maven-archetype-webapp`，创建一个Web Project

`-DinteractiveMode=`是否使用交互模式

`archetype`是mvn内置的一个插件，`create`任务可以创建一个java项目骨架，`DgroupId`是软件包的名称，`DartifactId`是项目名，`DarchetypeArtifactId`是可用的mvn项目骨架，

可以使用的骨架有：

```bash
maven-archetype-archetype  
maven-archetype-j2ee-simple  
maven-archetype-mojo  
maven-archetype-portlet  
maven-archetype-profiles (currently under development)  
maven-archetype-quickstart  
maven-archetype-simple (currently under development)  
maven-archetype-site  
maven-archetype-site-simple  
maven-archetype-webapp  
```

每一个骨架都会建相应的目录结构和一些通用文件，

最常用的是`maven-archetype-quickstart`和`maven-archetype-webapp`骨架。

`maven-archetype-quickstart`骨架是用来创建一个Java Project，

而`maven-archetype-webapp`骨架则是用来创建一个JavaWeb Project。

maven-project项目执行脚本

```bash
mvn archetype:generate -DgroupId=pers.ken.ssm.maven -DartifactId=maven-project -Dversion=0.0.1-SNAPSHOT -Dpackage=war -DarchetypeArtifactId=maven-archetype-webapp
```

1. 创建maven项目：`mvn archetype:create`
2. 指定 group： `-DgroupId=packageName`
3. 指定 artifact：`-DartifactId=projectName`
4. 创建web项目：`-DarchetypeArtifactId=maven-archetype-webapp`
5. 创建maven项目：`mvn archetype:generate`
6. 验证项目是否正确：`mvn validate`
7. maven 打包：`mvn package`
8. 只打jar包：`mvn jar:jar`
9. 生成源码jar包：`mvn source:jar`
10. 产生应用需要的任何额外的源代码：`mvn generate-sources`
11. 编译源代码： `mvn compile`
12. 编译测试代码：`mvn test-compile`
13. 运行测试：`mvn test`
14. 运行检查：`mvn verify`
15. 清理maven项目：`mvn clean`
16. 生成eclipse项目：`mvn eclipse:eclipse`
17. 清理eclipse配置：`mvn eclipse:clean`
18. 生成idea项目：`mvn idea:idea`
19. 安装项目到本地仓库：`mvn install`
20. 发布项目到远程仓库：`mvn:deploy`
21. 在集成测试可以运行的环境中处理和发布包：`mvn integration-test`
22. 显示maven依赖树：`mvn dependency:tree`
23. 显示maven依赖列表：`mvn dependency:list`
24. 下载依赖包的源码：`mvn dependency:sources`
25. 安装本地jar到本地仓库：`mvn install:install-file -DgroupId=packageName -DartifactId=projectName -Dversion=version -Dpackaging=jar -Dfile=path`

## web项目相关命令

1. 启动tomcat：`mvn tomcat:run`

2. 启动jetty：`mvn jetty:run`

3. 运行打包部署：`mvn tomcat:deploy`

4. 撤销部署：`mvn tomcat:undeploy`

5. 启动web应用：`mvn tomcat:start`

6. 停止web应用：`mvn tomcat:stop`

7. 重新部署：`mvn tomcat:redeploy`

8. 部署展开的war文件：`mvn war:exploded tomcat:exploded`
