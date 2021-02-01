---
title: wordpress 替换 google fonts 字体
siteurl: wordpress-替换-google-fonts-字体
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-853e'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-26T20:08:52+08:00'

---

google fonts 在国内部分地区的DNS无法正常解析，因为它们会将域名解析到美国那边的服务器ip，然后就完蛋了。

WordPress可以通过下面的办法来解决问题

虽然谷歌字体的服务解析在北京的服务器上，但是一些众所周知的原因，还是换了为好

因为你不确定你的用户在哪个地区 用着什么破网 访问你的服务器

## 以下三个办法任选其一

- 替换为 geekzu 库 <https://wordpress.org/plugins/useso-take-over-google/>
- 自己反代 <https://dallaslu.com/wordpress-mu-google-libraries-proxy/>
- 移除 Google Fonts <https://wordpress.org/plugins/remove-google-fonts-references/>
