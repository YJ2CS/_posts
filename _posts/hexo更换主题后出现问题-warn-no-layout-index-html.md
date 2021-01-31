---
title: hexo更换主题后出现问题：WARN No layout index.html
url: hexo更换主题后出现问题-warn-no-layout-index-html
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 博客
tags:
  - 博客
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-d7'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-17T00:19:33+08:00'

---


1.问题描述
hexo本地测试运行重启后页面空白,提示 : WARN No layout: index.html? 使用hexo clean 然后从新Generated再次运行还是空白

2.错误原因

## 缺少hexo插件

查看hexo插件安装情况
npm ls --depth 0

## 解决方法

安装插件
npm install xxx -s

# 主题名称和配置文件中不对应

运行git clone 指令获得主题后（假设是NEXT主题），在theme主题下保存文件夹的名称为：hexo-theme-next-0.4.0，如果在config里设置的是next，就会出现这样的WARN，http://localhost:4000/
显示的是空白。

## 解决方法

只要把theme下的文件夹名称改为正常的next就显示正常了。

