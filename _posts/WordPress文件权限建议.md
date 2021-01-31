---
title: WordPress文件权限建议
url: wordpress文件权限建议
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-3b54'
date: 2020-11-22T20:16:00.000Z
date updated: '2020-12-26T12:01:35+08:00'

---

### WordPress文件权限建议

| **相对路径**           | **建议** |
| ------------------ | ------ |
| /                  | 755    |
| wp-includes        | 755    |
| wp-admin           | 755    |
| wp-admin/js        | 755    |
| wp-content         | 755    |
| wp-content/themes  | 755    |
| wp-content/plugins | 755    |
| wp-content/uploads | 755    |
| wp-config.php      | 444    |
| .htaccess          | 444    |
| wp-content下的所有文件   | 644    |
| wp-includes下的所有文件  | 644    |

官方文档链接 [Changing File Permissions | WordPress.org - <https://wordpress.org/](https://wordpress.org/support/article/changing-file-permissions/)>

注意我不是虚拟主机，虚拟主机的权限要求会有不同,

大的权限设置方向如下：

1.对所有文件夹设置为755

2.对大部分文件设置为644

## 手动执行

你可以手动执行下面的全部命令

```shell

# WP_OWNER=www
# WP_GROUP=www
# WP_ROOT=/www/wwwroot/wordpress
# WS_GROUP=www

# reset to safe defaults
find /www/wwwroot/wordpress -exec chown www:www {} \;
find /www/wwwroot/wordpress -type d -exec chmod 755 {} \;
find /www/wwwroot/wordpress -type f -exec chmod 644 {} \;

# allow wordpress to manage wp-config.php (but prevent world access)
chgrp www /www/wwwroot/wordpress/wp-config.php
chmod 444 /www/wwwroot/wordpress/wp-config.php

# 更改.htaccess
chgrp www /www/wwwroot/wordpress/.htaccess
chmod 444 /www/wwwroot/wordpress/.htaccess

# allow wordpress to manage wp-content
find /www/wwwroot/wordpress/wp-content -exec chgrp www {} \;
find /www/wwwroot/wordpress/wp-content -type d -exec chmod 755 {} \;
find /www/wwwroot/wordpress/wp-content -type f -exec chmod 644 {} \;

# 修改wp-includes权限
find /www/wwwroot/wordpress/wp-includes -exec chgrp www {} \;
find /www/wwwroot/wordpress/wp-includes -type d -exec chmod 755 {} \;
find /www/wwwroot/wordpress/wp-includes -type f -exec chmod 644 {} \;

```

## sh脚本

例如,我使用宝塔面板安装的wordpress，设置情况是

WP_OWNER和WP_GROUP都是www

WP_ROOT是你的网站根目录，例如我的是/www/wwwroot/wordpress

WS_GROUP可以先查看一下，下面是查看命令

```shell
cat /etc/group
```

cat /etc/group查看所有组信息，如下图

[![linux如何查看所有的用户和组信息？](f31fbe096b63f62492d8448f8144ebf81b4ca3b1.jpg)](http://jingyan.baidu.com/album/a681b0de159b093b184346a7.html?picindex=3)

执行下载安装主题和插件的用户组是web用户组（一般为www）,

我们应该令WS_GROUP等于能够执行下载安装主题和插件的用户组。

我在查询以后确认我的web用户组是www，所以WS_GROUP=www

-   tips: 使用下面的命令可以修改用户组,将wordpress文件夹的用户和组全部修改成www

    ```shell
    cd 到wordpress文件夹的上一级文件夹
    chown -R www:www wordpress
    ```

总结就是：

```text
WP_OWNER=www
WP_GROUP=www
WP_ROOT=/www/wwwroot/wordpress
WS_GROUP=www
```

在服务器中新建文件fix.sh，内容是下面的部分.

之后使用命令执行它

```shell
cd 到fix.sh所在路径
sh fix.sh
```

```shell
#!/bin/bash
#
# This script configures WordPress file permissions based on recommendations
# from http://codex.wordpress.org/Hardening_WordPress#File_permissions
#
# Author: Michael Conigliaro <mike [at] conigliaro [dot] org>
#
# WP_OWNER=www-data # <-- wordpress owner
# WP_GROUP=www-data # <-- wordpress group
# WP_ROOT=$1 # <-- wordpress root directory
# WS_GROUP=www-data # <-- webserver group

WP_OWNER=www
WP_GROUP=www
WP_ROOT=/www/wwwroot/wordpress
WS_GROUP=www

# reset to safe defaults
find ${WP_ROOT} -exec chown ${WP_OWNER}:${WP_GROUP} {} \;
find ${WP_ROOT} -type d -exec chmod 755 {} \;
find ${WP_ROOT} -type f -exec chmod 644 {} \;

# allow wordpress to manage wp-config.php (but prevent world access)
chgrp ${WS_GROUP} ${WP_ROOT}/wp-config.php
chmod 444 ${WP_ROOT}/wp-config.php

# 更改.htaccess
chgrp ${WS_GROUP} ${WP_ROOT}/.htaccess
chmod 444 ${WP_ROOT}/.htaccess

# allow wordpress to manage wp-content
find ${WP_ROOT}/wp-content -exec chgrp ${WS_GROUP} {} \;
find ${WP_ROOT}/wp-content -type d -exec chmod 755 {} \;
find ${WP_ROOT}/wp-content -type f -exec chmod 644 {} \;

# 修改wp-includes权限
find ${WP_ROOT}/wp-includes -exec chgrp ${WS_GROUP} {} \;
find ${WP_ROOT}/wp-includes -type d -exec chmod 755 {} \;
find ${WP_ROOT}/wp-includes -type f -exec chmod 644 {} \;

```

## 修复脚本

错误修改导致服务器权限出现问题，可以使用下面脚本修复.

原理就是给与更高的权限.

在服务器中新建文件fix.sh，内容是下面的部分.

之后使用命令执行它

```shell
cd 到fix.sh所在路径
sh fix.sh
```

```sh
#!/bin/bash
#
# This script configures WordPress file permissions based on recommendations
# from http://codex.wordpress.org/Hardening_WordPress#File_permissions
#
# Author: Michael Conigliaro <mike [at] conigliaro [dot] org>
#
# WP_OWNER=www-data # <-- wordpress owner
# WP_GROUP=www-data # <-- wordpress group
# WP_ROOT=$1 # <-- wordpress root directory
# WS_GROUP=www-data # <-- webserver group

WP_OWNER=www
WP_GROUP=www
WP_ROOT=/www/wwwroot/wordpress
WS_GROUP=www

# reset to safe defaults
find ${WP_ROOT} -exec chown ${WP_OWNER}:${WP_GROUP} {} \;
find ${WP_ROOT} -type d -exec chmod 755 {} \;
find ${WP_ROOT} -type f -exec chmod 644 {} \;

# allow wordpress to manage wp-config.php (but prevent world access)
chgrp ${WS_GROUP} ${WP_ROOT}/wp-config.php
chmod 660 ${WP_ROOT}/wp-config.php

# 更改.htaccess
chgrp ${WS_GROUP} ${WP_ROOT}/.htaccess
chmod 660 ${WP_ROOT}/.htaccess

# allow wordpress to manage wp-content
find ${WP_ROOT}/wp-content -exec chgrp ${WS_GROUP} {} \;
find ${WP_ROOT}/wp-content -type d -exec chmod 775 {} \;
find ${WP_ROOT}/wp-content -type f -exec chmod 664 {} \;

# 修改wp-includes权限
find ${WP_ROOT}/wp-includes -exec chgrp ${WS_GROUP} {} \;
find ${WP_ROOT}/wp-includes -type d -exec chmod 775 {} \;
find ${WP_ROOT}/wp-includes -type f -exec chmod 664 {} \;

```

## 如何执行sh？

**第一种**（这种办法需要用chmod使得文件具备执行条件(x): chmod u+x datelog.sh）：

1、在任何路径下，输入该文件的绝对路径/root/datelog.sh就可执行该文件（当然要在权限允许情况下）

```shell
/root/datelog.sh
```

![img](20180128230149653.jpg)

2、cd到datelog.sh文件的目录下，然后执行

```shell
cd到datelog.sh文件的目录下
./datelog.sh
```

![img](20180128230209224.jpg)

**第二种**（这种办法不需要文件具备可执行的权限也可运行）：

1、在该文件路径,sh加上文件名字即可，

```shell
cd 该文件路径
sh datelog.sh
```

![img](20180128230242773.jpg)

2、在任意路径下，sh 加上文件路径及文件名称：

```shell
sh /root/datelog.sh
```

![img](20180128230303923.jpg)

## 源地址：

github gist: <https://gist.github.com/Adirael/3383404>

源文件链接： <https://gist.githubusercontent.com/Adirael/3383404/raw/2ce9e58cb48a3a85f4c8b667ebd3a42cdcda848b/fix-wordpress-permissions.sh>

embed:

<script src="https://gist.github.com/Adirael/3383404.js"></script>
