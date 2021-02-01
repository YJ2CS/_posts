---
title: Hexo NexT 加入背景图片
siteurl: hexo-next-加入背景图片
comments: true
categories:
  - 文章
tags:
  - 博客
author: WuGenQiang
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-c920'
date: 2019-03-31T13:21:57.000Z
updated: 2019-03-31T13:21:57.000Z
date updated: '2020-12-23T00:18:17+08:00'

---

## 添加如下代码：

给 hexo next 加上背景图片，只需要在 themes\next\source\css \ _custom\custom.styl 文件中

添加几行代码：

```css
@media screen and (min-width:1200px) {
    body {
    background-image:url(/images/background.jpg);
    background-repeat: no-repeat;
    background-attachment:fixed;
    background-position:50% 50%; 
    }
    #footer a {
        color:#eee;
    }
}
```

ok
