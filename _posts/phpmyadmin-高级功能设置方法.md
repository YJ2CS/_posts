---
title: phpMyAdmin 高级功能设置方法
url: phpmyadmin-高级功能设置方法
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-87db
date: 2020-11-16 17:54:00
---


[phpMyAdmin 高级功能设置方法](https://teddysun.com/268.html)
!!! 按照官方推荐，在升级phpMyAdmins时，您应该将旧的phpMyAdmin删除，升级安装成新版本后，重新配置高级功能，使用宝塔面板则要求您删除旧的phpMyAdmin,之后才能更新成新版。
```text
首先删除仅保留配置的旧文件。
```

```text
从旧版本升级
警告切勿从现有的phpMyAdmin (即phpmyadmin-old) 安装中提取新版本，始终首先删除旧文件，仅保留旧的配置文件。（译者注：config.inc.php）
这样，您将不会在目录中保留任何旧文件或过时文件，因为这可能会严重影响安全性或导致各种损坏。

只需将您以前的安装复制到新包装中即可。旧版本的配置文件可能需要进行一些调整，因为某些选项已更改或删除。为了与PHP 5.3及更高版本兼容，请删除一条可能在配置文件末尾找到的语句。config.inc.phpset_magic_quotes_runtime（0）;

您不应复制覆盖，因为默认配置文件是特定于版本的。参加 libraries/ config.default.phpconfig.inc.php

只需几个简单的步骤即可完成完整的升级：

从<https://www.phpmyadmin.net/downloads/>下载最新的phpMyAdmin版本。
重命名现有的phpMyAdmin文件夹（例如phpmyadmin-old）。
将最新下载的phpMyAdmin解压缩到所需位置（例如phpmyadmin）。
将文件 .config.inc.php 从旧位置（phpmyadmin-old）复制到新位置（phpmyadmin）
(译者注:!!!旧版本的配置文件可能需要进行一些调整，因为某些选项已更改或删除。!!!
关于哪些配置选项已经更改或者删除，请参见 config.sample.inc.php 中的范例。)

测试一切正常。
删除旧版本的备份（phpmyadmin-old）.

如果已将MySQL服务器从4.1.2之前的版本升级到5.x或更高版本，并且使用phpMyAdmin配置存储，则应运行.sql / upgrade_tables_mysql_4_1_2 + .sql中的SQL脚本。

如果您已将phpMyAdmin从2.5.0或更高版本升级到4.3.0或更高版本（<= 4.2.x），并且使用phpMyAdmin配置存储，则应运行.sql / upgrade_column_info_4_3_0 + .sql中的SQL脚本。

不要忘记清除浏览器缓存并通过注销并再次登录来清空旧会话。
```
原文如下:
```text
Upgrading from an older version
Warning Never extract the new version over an existing installation of phpMyAdmin, always first remove the old files keeping just the configuration.
This way, you will not leave any old or outdated files in the directory, which can have severe security implications or can cause various breakages.

Simply copy from your previous installation into the newly unpacked one. Configuration files from old versions may require some tweaking as some options have been changed or removed. For compatibility with PHP 5.3 and later, remove a statement that you might find near the end of your configuration file.config.inc.phpset_magic_quotes_runtime(0);

You should not copy over because the default configuration file is version- specific.libraries/config.default.phpconfig.inc.php

The complete upgrade can be performed in a few simple steps:

Download the latest phpMyAdmin version from <https://www.phpmyadmin.net/downloads/>.
Rename existing phpMyAdmin folder (for example to ).phpmyadmin-old
Unpack freshly downloaded phpMyAdmin to the desired location (for example ).phpmyadmin
Copy from old location () to the new one ().config.inc.php`phpmyadmin-oldphpmyadmin
Test that everything works properly.
Remove backup of a previous version ().phpmyadmin-old
If you have upgraded your MySQL server from a version previous to 4.1.2 to version 5.x or newer and if you use the phpMyAdmin configuration storage, you should run the SQL script found in .sql/upgrade_tables_mysql_4_1_2+.sql

If you have upgraded your phpMyAdmin to 4.3.0 or newer from 2.5.0 or newer (<= 4.2.x) and if you use the phpMyAdmin configuration storage, you should run the SQL script found in .sql/upgrade_column_info_4_3_0+.sql

Do not forget to clear the browser cache and to empty the old session by logging out and logging in again.
```

phpMyAdmin 安装后，默认其高级功能是不开启的，所以一般登录到 phpMyAdmin 后，会提示
```text
“phpMyAdmin 高级功能尚未完全设置，部分功能未激活。请点击这里查看原因。”。
```
而所谓的高级功能，其实就是存储 phpMyAdmin 的各种参数到数据库中。
　　要解决这个问题也不难，实际上根据 phpMyAdmin 的提示一步一步也能完成。这里简单记录一下过程。

　　第一步，在 phpMyAdmin 源码的 examples 目录（5.0后在sql目录）下有个 create_tables.sql 文件，这就是创建名为 phpmyadmin 数据
库的SQL文。当你用 root 用户登录 phpMyAdmin 后，在“导入”页面，上传这个 create_tables.sql 文件即可成功创建数据库phpmyadmin。
![phpmyadmin-1.png](images/phpmyadmin-1.png)

　　第二步，创建完数据库 phpmyadmin 后，展开左侧phpmyadmin，出现12张表名。

![phpmyadmin-2.png](images/phpmyadmin-2.png)

　　第三步，更改Linux服务器中安装目录下的配置文件 (config.inc.php)中参数，参见 config.sample.inc.php 中的范例。有关
 phpMyAdmin configuration storage settings 的设置如下：（随版本更新会发生变化，
 
 但总的来说，一定要把 `Storage database and tables` 下的选项全部开启）

```html
$cfg['Servers'][$i]['pmadb'] = 'phpmyadmin';
$cfg['Servers'][$i]['bookmarktable'] = 'pma__bookmark';
$cfg['Servers'][$i]['relation'] = 'pma__relation';
$cfg['Servers'][$i]['table_info'] = 'pma__table_info';
$cfg['Servers'][$i]['table_coords'] = 'pma__table_coords';
$cfg['Servers'][$i]['pdf_pages'] = 'pma__pdf_pages';
$cfg['Servers'][$i]['column_info'] = 'pma__column_info';
$cfg['Servers'][$i]['history'] = 'pma__history';
$cfg['Servers'][$i]['tracking'] = 'pma__tracking';
$cfg['Servers'][$i]['designer_coords'] = 'pma__designer_coords';
$cfg['Servers'][$i]['userconfig'] = 'pma__userconfig';
$cfg['Servers'][$i]['recent'] = 'pma__recent';
$cfg['Servers'][$i]['table_uiprefs'] = 'pma__table_uiprefs';
```

　　第四步，退出，并重新登录 phpMyAdmin 以加载新配置并使其生效。

　　需要注意的是，
  
  在更新版本后，旧版本的配置文件可能需要进行一些调整，因为某些选项已更改或删除。
  
  数据库也可能会更改或删除。例如，我记得老版本中的 create_tables.sql 创建出的表名，类似于 `pma_bookmark` ，是一条下划线 `_` .
  
而新版本对应的是 `pma__bookmark` ,两条下划线  `__` ，因此配置文件中也要做出相应的更改。方法是：

在examples文件夹或者sql文件夹下有upgrade_tables_4_7_0+.sql。运行它。

或者自己手动更改。

更建议的操作是删除旧的数据库 `phpmyadmin` ，重新创建。

具体方法参考上面的英文和译文。
