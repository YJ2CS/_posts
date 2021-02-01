---
title: Hexo之修改内容区域的宽度
siteurl: Hexo之修改内容区域的宽度
comments: true
categories:
  - 文章
tags:
  - 博客
author: WuGenQiang
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-6fa6'
date: 2019-04-05T23:20:27.000Z
updated: 2019-04-05T23:20:27.000Z
date updated: '2020-12-23T00:16:38+08:00'

---

# 1 修改内容区域的宽度

编辑主题的 source/css/_variables/custom.styl  文件，新增变量：

```js
// 修改成你期望的宽度
$content-desktop = 700px
// 当视窗超过 1600px 后的宽度
$content-desktop-large = 1050px
```
