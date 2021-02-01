---
title: wp-super-cache-fastcgi-cache-应该选哪个
siteurl: wp-super-cache-fastcgi-cache-应该选哪个
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-wp-super-cache-fastcgi-cache-应该选哪个'
date: 2020-12-18 22:47:55
date updated: '2020-12-18T22:50:32+08:00'
---

[[WordPress插件选择]]

WP Super Cache和fastcgi cache都是把动态页面（php）伪静态化（HTML），即进行缓存（cache）
两者都是用来节省服务器生成动态页面的开支的。

我查了许久文献。没有发现他们之间存在什么冲突。

但两者功能毕竟相同，对于缓存器来说，保留其中之一就可以了。

fastcgi cache肯定要比 wp super cache更快的，因为前者是nginx层面的缓存。

proxy cache，顾名思义。它只在你的网站使用了代理（proxy）时工作，通过缓存来解决代理带来的巨量服务器开支。

proxy有很多种，但我们实际上只有在跨IP，跨域名的代理上需要使用proxy cache。其余情况下，你配置不会有坏处。只是他不会工作。

Jetpack CDN由于它在前台需要加载链接WordPress.org的js、css 所以事实上在中国地区的服务器是负优化。
