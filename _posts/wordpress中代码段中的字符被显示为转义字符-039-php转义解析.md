---
title: Wordpress中代码段中的字符被显示为转义字符&#039 PHP转义解析
url: wordpress中代码段中的字符被显示为转义字符-039-php转义解析
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-c740
date: 2020-11-16 17:54:00
---
wordpress中代码段中的字符被显示为转义字符.
支持markdown的主题与markdown插件联用时，代码段中的字符被显示为转义字符：&转义为 & 中括号< 、>以及单双引号'、“被转义成'debug时发现将decode_code_blocks启用（修改false为true）后问题解决。

## 解决方法

首先卸载所有markdown编辑器相关的markdown插件。
然后下载一个WP Githuber MD插件。
然后在插件设置里的偏好设定中，选择"清除所有设置"选项。

![file](images/image-1604815149852.png)

然后将WP Githuber MD插件卸载
然后将WP Githuber MD插件安装回来。
然后在偏好设定里，开启“解码程式码区块”功能，即issue#89相关的功能。

然后将你内容有问题的文章重新"HTML转换成Markdown"，并且发布更新。

# 原理

Sakura主题与现有的markdown插件冲突，导致代码块中符号<> “”'等被转义。
根据[issue#89](https://github.com/terrylinooo/githuber-md/issues/89)描述的问题和解决方案。

![file](images/image-1604815347488.png)

Sakura主题默认禁用了/Controllers/Markdown.php中第1041行的decode_code_blocks，将其值默认设置为false

```PHP
'decode_code_blocks' => false, // Fix: issue #30
```

我们需要手动将其值修改为true

```php
'decode_code_blocks' => true, // Fix: issue #30
```

而WP Githuber MD插件给我们提供了一个快捷的按钮，不用手动修改源码，只需要点击这个开关

# 需要卸载重装

在插件设置里的偏好设定中，选择"清除所有设置"选项。然后将WP Githuber MD插件卸载并重新安装回来，是为了解决由于用户误操作导致开启了WP Githuber MD插件中的一些选项，导致出现未知bug，从而本文不起作用的问题。

# WP Githuber MD插件原理解析

WP Githuber MD插件中，源文件HTML和转换后的markdown文件分别保存在了两个不同的栏位中，这样清除WP Githuber MD插件数据并不会对源文件HTML产生影响，我们只需要一步卸载重装(选择"清除所有设置"选项。)的操作，就可以把之前错误转换来的markdown文件全部清除(*并不会对源文件HTML产生影响*)。之后在偏好设定里，开启“解码程式码区块”功能，新转换来的markdown文件就正常了
