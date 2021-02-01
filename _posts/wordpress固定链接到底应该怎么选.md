---
title: Wordpress固定链接到底应该怎么选?
siteurl: wordpress固定链接到底应该怎么选
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-42b3'
date: 2020-11-22T20:16:00.000Z
date updated: '2020-12-26T19:54:58+08:00'

---

首先，想要对搜索引擎友好（SEO优化），固定链接一定要浅(`/year/month/day/postname`这种就属于太深了，对搜索引擎很不友好)，经验上，深度不要超过3，即最多`/catg/sub-catg/postname`这样的深度.

同时可以自选是否使用中文做固定链接，这有利有弊吧，有利的是某些搜索引擎会中文url加粗，弊就是会自动转码，转码过后链接太长了，不利于传播

结论，经过测试，想要SEO友好，有以下几种推荐设置

如：网址/120.html，简洁，带html结尾的伪静态，需要服务器URL_Rewrite支持。

```text
/%post_id%.html
```

如：网址/themebetter-is-ok，语义化文章别名的伪静态，需要服务器URL_Rewrite支持。

```text
/%postname%
```

而在此基础上，

- 想要对编辑者和网页浏览者友好，选择`/%postname%`
- 想要适配当前主流的第三方发布插件(各种兼容xmlrpc.php的文章发布软件)，选择`/%postname%`
- 同时对`类markdown`语法友好，和对`yaml语法`友好，两种设置都ok

综合选择就只剩下以下的推荐设置

```text
/%postname%
```

所以结论就是，别纠结了，就选这种吧：

## 对`类markdown`语法友好

在现在的主流markdown渲染引擎中，有一项功能称为`linkify`，这项功能一般包括在渲染引擎的渲染器(`render`)中，

当这项功能开启时，一些 `像URL的文本或链接` (原话是`like url`)会被渲染成`URL`

比如现在markdown有一种比较主流的扩充语法(extra)，即使用 `[[title_name]]`（两个[]嵌套）,解析后,指向另外一篇文章,这被广泛用在一些文档管理系统中，主流的markdown引擎都对它支持的比较好。

在我们预期中预期，一篇文章发布到博客后，其中的`[[title_name]]`应当也被自动解析并渲染成指向另外一篇文章的URL，

这时候`linkify`这项功能就起作用了，其开启后，就能达到我们预期的效果。

重点来了，要注意，使用`/%postname%/`这种链接后，`linkify`就不能解析和渲染出我们想要的结果.

它会将 `[[title_name]]` 使用类似解析文章内图片的方式进行解析.即使用`/%postname%/`这种链接后,

解析成:`http://youdomain.com/current_article/title_name/`, 会把`title_name`当成某个文件夹来处理,达不到我们想要的结果

使用`/%postname%`这种链接后，相应的会解析成:`http://yourdomain.com/title_name`，这符合我们预期的结果，

## WordPress固定链接自定义时出现404：

自定义好的固定链接访问后出现404，多是服务器没有开启URL_Rewrite的支持，所以先去服务器设置或者找你的主机商寻求帮助。

### Apache环境下开启url_rewrite：

- 开启apache的url_rewrite模块，也就是在httpd.conf中去掉这句话的注释LoadModule rewrite_module modules/mod_rewrite.so
- 找到AllowOverride，把AllowOverride None修改成AllowOverride all
- 在所需要进行rewrite的web的主目录下添加.htaccess文件，添加上一句话：RewriteEngine on

### Nginx环境下开启url_rewrite：

- nginx只需要打开nginx.conf配置文件，在server里面写需要的规则，然后重启即可。

- 具体的重写规则参考：http://codex.wordpress.org/Nginx

WordPress固定链接小提示：

```text
固定链接最好是在建站时就定好；
如果后期变更固定链接一定要做好301跳转，可以搜索选择Redirection插件来解决；
固定链接本身并不能达到很科学的SEO效果，各种方式并无区别；
不要纠结固定链接是个什么样子。
```

至此，WordPress固定链接设置上的问题都被解决了。

本文属原创，转载请注明原文：https://themebetter.com/wordpress-link-404.html

- 相关文章： [Java：URL rewrite](https://www.oxysun.cn/java/url-rewrite.html)

## 在hexo中

不带后缀名会导致有的web server(比如常见的GitHub pages)发送错误的MIME类型，浏览器接收到非HTML的MIME，会默认进行下载

所以支持带一个`/`后缀，或者以.html为后缀，需要有后缀

具体到对`类markdown语法`友好，那么就选择以.html为后缀

深层次原因可能是hexo部署的服务器url_rewrite功能设置有问题，

由于hexo通常是部署在GitHub pages上的，问题也经常在GitHub pages上出现问题，所以这个问题应该是GitHub那边的主机问题，这是服务器环境配置上的问题。
