---
title: 如何在HEXO中插入图片
siteurl: 如何在HEXO中插入图片
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-如何在HEXO中插入图片
date: '2021-01-12T22:46:35+08:00'
date updated: '2021-01-12T22:46:38+08:00'

---

> Many a false step was made by standing still.
> — <cite>Fortune Cookie</cite>

在用MarkDown进行文章撰写的时候，本地的图片插入相对比较方便，但是类似HEXO这样把MarkDown文件转成静态网页文件再发布，因为牵扯到链接图片文件的路径问题，有点麻烦。如何方便实现在HEXO中插入图片呢？

### HEXO官方解决方案

#### 少量图片的简便办法

对于零星少量的图片使用，可以在网站根目录下的`source`文件夹中建立一个`images`文件夹，将所需要的图片文件复制到此文件夹，使用MarkDown中`![](/images/image.jpg)`的方式访问使用。

#### 大量或者规律性使用图的办法

对于大量或者规律性使用图片的，HEXO提供了另外一种结构化的图片资源管理。就是将`_config.yml`配置文件中`post_asset_folder: false` 改为 `post_asset_folder: true`。这个开关打开后，每次`hexo new`命令会生成一个同名同位置的文件夹，用以存放图片资源。当然这个文件夹中也可以存放类似图片这样需要位置引用的资源。在HEXO发布的时候，这个文件夹中的内容会随同文章内容一并上传发布。这样也可以使用MarkDown中`![](/image.jpg)`的方式访问使用。

#### 相对路径访问图片等资源问题和解决办法

通过常规的MarkDown语法和相对路径引用图片或其它资源相对方便，但是这些包含相对位置引用资源的文章在首页，或者存档页面不能正常显示。HEXO 3开始核心代码中加入了相对路径引用的标签插件，用以解决这个问题。（在HEXO 3之前许多开发者开发类似插件，现在被官方更新后核心代码采用了）。

```text
{% asset\_path slug %}  
{% asset\_img slug \[title\] %}  
{% asset\_link slug \[title\] %}  
```

#### 示例

1. 网站根目录source文件夹图片MarkDown常规引用。`![Source Image](/images/img-in-source.jpg)`

![Source Image](https://iworld2u.com/images/img-in-source.jpg)

2. 本文同名文件夹图片MarkDown常规引用。`![Post Image](img-in-post.jpg)`

![Post Image](https://iworld2u.com/2020/03/02/How-to-insert-images-in-hexo/img-in-post.jpg)

3. 使用标签插件的引用。`{ % asset_img img-in-post.jpg Post Image % }`

Post Image

![](https://iworld2u.com/2020/03/02/How-to-insert-images-in-hexo/img-in-post.jpg "Post Image")

### 其它的解决方案

如果有图床的话，使用常规MarkDown语法采用绝对位置访问。或者，安装那些大牛们的插件，按照他们的套路来。

- **本文作者：** Joe Wang
- **本文链接：** [http://iworld2u.com/2020/03/02/How-to-insert-images-in-hexo/](http://iworld2u.com/2020/03/02/How-to-insert-images-in-hexo/ "如何在HEXO中插入图片？")
- **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/zh-CN) 许可协议。转载请注明出处！
