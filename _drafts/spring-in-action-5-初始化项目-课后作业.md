---
title: 学习 Spring Boot 实战第5版：初始化项目 课后作业
url: spring-in-action-5-初始化项目-课后作业
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
no-photos: >-
  https://random.52ecy.cn/randbg.php?size=1&rid-spring-boot-in-action-learning-初始化项目-课后作业
date: '2021-01-27T11:31:45+08:00'
date updated: '2021-01-27T11:48:18+08:00'

---

> The best way to not feel hopeless is to get up and do something. Don't wait for good things to happen to you. If you go out and make some good things happen, you will fill the world with hope, you will fill yourself with hope.
> — <cite>Barack Obama</cite>

这里我将按照本节课要求，初始化一个名为 Pussy Cloud 的项目

> 不管黑 pussy 、白 pussy，能够抓到老鼠就是好pussy

![[Pasted image 20210127113816.png]]
![[Pasted image 20210127114039.png]]
![[Pasted image 20210127114117.png]]
![[Pasted image 20210127114205.png]]

运行一下PussyCloudApplicationTests，没有问题
![[Pasted image 20210127114434.png]]

## 编写Spring应用

### 添加控制器类

在`com/pussycloud/core/web/`下新建一个`HomeController.java`

使用Thymeleaf模板定义视图
