---
title: WordPress静态缓存插件WP Super Cache的使用方法
siteurl: wordpress静态缓存插件wp-super-cache的使用方法
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-b18a
date: 2020-11-22 20:16:00
---
[[WordPress插件选择]]

转载自：<http://www.shaofee.com/archives/347.html>

WP Super Cache是众多静态缓存插件中最值得推荐的一款，下面将着重介绍他的使用方法。
第一步：安装WP Super Cache
为节省时间，此步骤省略。不会安装WordPress插件的可以自行百度，非常简单。
第二步：配置WP Super Cache
1、文件夹权限设置
将wp-content目录权限设置为读写权限，因为后面生成的缓存文件会放在次目录下，可使用FTP工具设置该文件夹属性的权限值为755；
将WordPress根目录下的wp-config.PHP和.htaccess两个文件的权限同样设置为可写权限，同时备份好这两个文件，因为此插件会修改这两个文件的内容，如果后面不使用此插件了，可以恢复备份的这两个文件。
2、配置插件
进入插件设置置页面，勾选所有带有“推荐”字样的选项，其他选项保持默认即可。
"当某页面有新评论时，只刷新该页面的缓存" 这一项也要选。
然后点击更新按钮

3、设置完毕后，点击更新按钮，会提示你点击“更新 Mod_Rewrite 规则”按钮，向下滚动找到该按钮并点击。

插件会自动向Wordpress根目录的wp-config.php和.htaccess文件写入相关规则。

## 清理缓存及停用插件

1、清理缓存

可以定期手动清理缓存文件，打开WP Super Cache插件设置页面，点击“内容”选项卡，点击“删除缓存”。

2、停用插件

重复上一步（必须），之后点击“高级”选项卡，取消“启用缓存以便加快访问。 (推荐)”勾选，并点击下面的“更新”按钮，

3、完全删除插件

重复上面两步（必须），然后进入插件页面停用WP Super Cache插件，并删除。

正常情况下删除WP Super Cache插件时，会将之前插件所修改和创建的缓存文件夹一并删除，但也可能有例外。所以，登录Ftp客户端，用之前备份的wp-config.php和.htaccess文件覆盖Wordpress根目录的同名文件，并删除wp-content目录的cache文件夹，这样才能完全卸载并彻底删除缓存文件。

另外，建议安装网页压缩插件：Autoptimize与WP Super Cache配套使用，可以进一步加快网页打开速度。

设置Autoptimize插件时，只需要勾选“优化 HTML 代码和优化 CSS 代码”，其它默认即可，不要勾选“优化 [JavaScript](http://lib.csdn.net/base/javascript) 代码”否则可能造成主题部分功能不可用，切记！
