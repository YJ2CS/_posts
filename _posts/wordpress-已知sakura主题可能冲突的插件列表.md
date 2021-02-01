---
title: WordPress 已知Sakura主题可能冲突的插件列表
siteurl: wordpress-已知sakura主题可能冲突的插件列表
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 博客
tags:
  - 博客
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-ee38'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-26T20:07:41+08:00'

---

[[WordPress插件选择]]

[toc]

## 已知Sakura主题可能冲突的插件列表

Sakura是一个强大的一键式个人博客功能整合主题。它提供了许多原生WordPress需要插件才能实现的功能。这也就意味着它会与许多插件的功能重复，从而造成相互冲突、过度渲染、数据库崩溃、PHP致命错误等等问题。本文会列出一些已知的相互冲突的插件，以及个别可共存插件但是需要注意的具体选项。

## BuddyPass

页面重复渲染、小概率页面冲突、首页样式覆盖

## WP Githuber MD

第一页的功能可以正常使用.
需要注意的是第二页:
代码高亮，两种方式都与Sakura主题冲突，会重复渲染。
复制到剪贴板功能与Sakura主题重复，会出现两个控件，极不美观
图片粘贴可以正常使用，而且推荐打开。
内容纲要就在页面前的

```text
[ toc ]
```

试过就知道是什么，与Sakura主题重复
Emojify功能与Sakura主题重复，且打开后会引起PHP致命错误，只能在后台删除插件解决，千万不要打开
其余功能还没测试过，不予置评
第三页功能都无效，因为Sakura主题会覆盖这些设置，实际上他们并不能起作用
第四页中一定要打开解码程式码区块功能，这个功能是修复与Sakura主题联用会出现的一个字符转义问题，请看这篇: [[wordpress中代码段中的字符被显示为转义字符-039-php转义解析]]
智慧型引号功能是wordpress自带的，如果你不清楚他是什么，就不要去关闭。
其余功能我打开了一个短代码，剩下的没有测试

## WPForm

重复渲染，与Sakura主题功能重复，而且因为我是个人博客，没有详细测试所有功能，建议不要启用

## WP Editor.md

作者称其在WordPress5.3版本中表现较差，会与Sakura主题冲突，但我实测中还没发现问题

| Wordpress中代码段中的字符被显示为转义字符`&#039` PHP转义解析 | [[wordpress中代码段中的字符被显示为转义字符-039-php转义解析]] |
