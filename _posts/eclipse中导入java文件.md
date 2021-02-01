---
title: eclipse中导入Java文件的方法
siteurl: eclipse中导入java文件
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - IDE
tags:
  - IDE
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-f488'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T20:12:45+08:00'

---

# eclipse中导入Java文件的方法

1，如果要导入的Java文件就在eclipse工作空间WorkSpace目录下，则把包含相关Java文件的Java项目导入包资源管理器即可：

文件（或者包资源管理器下点击右键）---->导入---->常规--->现有项目到工作空间--->在 选择根目录 下浏览选择WorkSpace中包含相关Java文件的Java项目，其他不用勾选，点击完成即可；

2，如果要导入的Java文件在别的目录下，则

新建一个Java项目如Hello,复制要导入的.java文件，把Hello打开，然后在src下点右键粘贴就可以正常运行了；如果直接在Hello下粘贴，打开后会发现Java文件跟在了JRE目录下了，然后运行会出现错误：编辑器未找到main类型。

最后呢，导入的文件就在新的目录下工作运行了（即WorkSpace下了），eclipse中对文件的修改就不会改动到别的目录下的相同文件了。
