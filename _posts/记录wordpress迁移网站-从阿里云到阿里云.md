---
title: 记录wordpress迁移网站，从阿里云到阿里云
url: 记录wordpress迁移网站-从阿里云到阿里云
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-626b'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-26T20:09:24+08:00'

---

写在最前面：绝大多数新购买的空间，都会提供一次性的迁移服务，他们的技术人员会帮你免费迁移网站，在查看本教程之前，可以先问下空间的客服，看能不能免费迁网站，如果可以，那就不必花时间读这篇文章了。

以下正文：

---

很多新手对迁移WordPress网站感到畏惧，因为听上去就很麻烦，但作为几乎隔几天就要自己迁一次网站，或帮朋友迁移一次网站的人来说，迁移网站比出门买杯咖啡还要轻松。

迁移WordPress网站很简单，只需要以下几个步骤：

1. **在旧网站导出数据包；**
2. **把域名转移到新空间；**
3. **在新空间安装WordPress；**
4. **在新网站导入数据包；**
5. **保存固定链接并检查全站。**

以上留个步骤适用于任何WordPress空间和网站，哪怕你的空间是用VPS搭建的，也同样可以用这个方法完成迁移。

这篇文章我会就以下大纲详细展开说明：

## 用All-in-One插件导出数据包

在淘宝买卖东西，大体流程是，首先卖家把商品打包好，然后卖家把东西发出去，买家收到货之后再拆快递，这样就完成了整个交易。

WordPress迁移网站也是一样，也是“打包→发货→收货→拆快递”这个过程，所以第一步就是把旧网站的内容都打包好。

WordPress有个傻瓜式的一键发货插件，叫作[All-in-One WP Migration](https://cn.wordpress.org/plugins/all-in-one-wp-migration/)，在WordPress的“插件→安装插件”搜索就能找到：

![image-116-1024x419.png](image-116-1024x419.png)

然后WordPress后台左侧，就会多出一个叫“All-in-One WP Migration”的菜单，我们点击菜单当中的“导出”：

![image-117-1024x548.png](image-117-1024x548.png)

再下一步，你会看到有多种导出的方式，最普通的就是选“文件”，这样我们就可以把数据包下载到我们自己电脑上。

![image-119-1024x556.png](image-119-1024x556.png)

这个教程我以最平常的手段来做，我们选择“文件”，将整个数据包下载到本地：

![image-118-1024x530.png](image-118-1024x530.png)

点击之后它会自动读取网站的数据，准备打包，这一步要等待几分钟：

![image-120-1024x394.png](image-120-1024x394.png)

![image-121-1024x377.png](image-121-1024x377.png)

等插件打包好网站所有的文件和数据库之后，就会是下面这个提示，点击下载，就能把数据包下载到自己电脑：

![image-122-1024x356.png](image-122-1024x356.png)

## 保存旧网站的关键信息

## WordPress信息

管理员账号：

admin

### 管理员密码

    **********

请复制如下命令并登陆服务器，鼠标右键-粘贴执行

sudo grep wordpress_admin_passwd /root/env.txt

首页地址：

[http://47.95.207.150](http://47.95.207.150/)

管理员登录地址：

<http://47.95.207.150/wp-login.php>

网站根目录：

/home/www/htdocs

## MySQL信息

数据库账号：

root

### 数据库密码

    **********

请复制如下命令并登陆服务器，鼠标右键-粘贴执行

sudo grep mysql_root_passwd /root/env.txt

数据库地址：

127.0.0.1:3306

数据库名称：

wordpress

## 把域名转移到新空间

我没有域名，跳过这步

<https://www.xunpanziyou.com/wordpress-migration/>

首先在新空间上新建一个网站，这里以Hostinger为例。

进入后台后点击导航上的“Hosting”，然后点击“Add Website”：

注意，每个空间新建网站的方式不一样，具体可以谷歌“How to add a new website on xxx”。

![image-126-1024x374.png](image-126-1024x374.png)

点击之后输入老域名，设置个密码（此处的密码无关紧要）即可：

![image-127-1024x391.png](image-127-1024x391.png)

之后要管理网站，只需点击导航条的Hosting，就能找到对应域名的“Manage”：

![image-128-1024x373.png](image-128-1024x373.png)

现在我们就相当于跟新空间就“打好招呼”了，接下来就是把域名的控制权也转过来。

要转移域名控制权，只要修改下DNS即可，首先进入我们购买域名的地方，比如我的域名是在[NameSilo](https://www.namesilo.com/)上买的，那就到后台修改DNS：

![image-132-1024x363.png](image-132-1024x363.png)

每个平台修改DNS的地方不一样，如果找不到修改的地方，可以在谷歌搜索“How to change DNS in xxx”即可，比如我搜“How to change DNS in namesilo”，就能搜到一大堆教程：

![image-124-1024x568.png](image-124-1024x568.png)

我们需要把域名的DNS信息，改成新空间指定的DNS即可，每个空间的DNS信息不一样，还是谷歌大法，可以搜索：where can I find my nameservers on xxx，这里以Hostinger为例：

![image-125-1024x522.png](image-125-1024x522.png)

比如我要把网站搬迁到[Hostinger](https://hostinger.com/imiker)上，我只需要根据以上教程，进入到域名的管理页面：

![image-129-1024x373.png](image-129-1024x373.png)

选择截图中的Details：

![image-130-1024x463.png](image-130-1024x463.png)

就能找到我这个域名的NameServe应该是ns1.dns-parking.com和ns2.dns-parking.com。

![image-131-1024x488.png](image-131-1024x488.png)

回到NameSilo的后台，将这两个地址填进去，保存即可。

![image-133-1024x429.png](image-133-1024x429.png)

完成之后这一整个步骤就搞定了，接下来就要等2-24小时，等域名的DNS信息在全球范围内更新。

## 新服务器重置为centos8.X

![image-20201117124519241.png](image-20201117124519241.png)

## 新服务器使用宝塔安装wordpress

首先安装宝塔:

## 宝塔官网

<https://www.bt.cn/>

[宝塔Linux面板安装教程 - 2020年8月28日更新 - 7.4.5正式版](https://www.bt.cn/bbs/thread-19376-1-1.html)

安装命令

    yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

## 使用宝塔安装wordpress

<https://developer.aliyun.com/article/752117>

记录下你安装后默认宝塔的内网外网账号密码

我们搭建wordpress是基于php的，为了稳定，我们选择lamp(apache)

网站域名，由于没有域名，使用默认的ip地址

<https://zouaw.com/8900.html>

![image-20201117132158699](image-20201117132158699.png)

## 在新网站导入数据包

打开网站后台之后，点开“插件→安装插件”，安装最开始我们提到的All-in-One插件：

![image-142-1024x512.png](image-142-1024x512.png)

然后点击“导入”：

![image-143-1024x552.png](image-143-1024x552.png)

如果你的数据包不到128MB，这一步就可以直接选“FILE”导入，如果你和我一样数据包比较大，那就要额外多安装截图中的插件：

![img](image-144-1024x472.png)

点击[这个链接](https://import.wp-migration.com/all-in-one-wp-migration-file-extension.zip)可以直接下载对应插件，下载好之后，在后台通过“Plugin→Add New→Upload Plugin”，上传激活即可：

![img](image-145-1024x422.png)

![img](image-146-1024x479.png)

激活之后就能继续上传了，点击“FILE”选择之前导出来的数据包：

![img](image-147-1024x542.png)

最后静静等待进度条到100%：

![img](image-148-1024x406.png)

进度条满了之后会出现这个界面，选择右边的PROCEED：

![img](image-149-1024x448.png)

继续等待直到截图中的数字变成100%：

![img](image-150.png)

完成之后是下面这样的界面，看到这个页面就可以关掉网站，重新从空间登录网站后台了：

![img](image-151-1024x409.png)

## 保存固定链接并检查全站

再次进入后台，你会发现网站已经迁移成功了，不过还需要保存下固定链接，否则会出现只有首页正常，其他页面都打不开的情况。

稍微讲下原理：这是因为All-in-One插件虽然能帮你迁移网站的文件和数据库，但有个关键的文件.htaccess它没法自动生成，而这个文件是负责整个网站URL rewrite的，而我们在后台保存固定链接，就能重新生成.htaccess，让网站正常工作。

进入WordPress后台的设置→固定链接（Permalink），直接点击最下方的“保存”即可：![img](image-153-1024x550.png)到这一步，网站就完全迁移成功了！最后再彻底检查下网站各个页面是否正常（一般都正常，极少出问题），我们的任务就Over了。

我们的wordpress admin登录密码使用之前旧网站的admin密码

## 部署memcached

### 安装memcached

Ps：这里的memcached是指Mencached的服务端，用来处理缓存数据，名字也是容易混淆。

### 集成php-memcached拓展

**先安装libmemcached**

**安装php-memcached组件**

## 配置清理Memcached缓存

<https://blog.naibabiji.com/skill/wordpress-qing-li-memcached-huan-cun.html>

<https://my.oschina.net/u/4581486/blog/4372170>

<https://aliyun.gaomeluo.com/427.html>

<https://aliyun.gaomeluo.com/435.html>

## WordPress缓存

做完上述所有步骤，系统环境就已经支持memcached缓存了。下面分享如何应用到WordPress

### 1、安装插件

    访问github项目页面下载插件包：

    [https://github.com/tollmanz/wordpress-pecl-memcached-object-cache](https://zhang.ge/goto/aHR0cHM6Ly9naXRodWIuY29tL3RvbGxtYW56L3dvcmRwcmVzcy1wZWNsLW1lbWNhY2hlZC1vYmplY3QtY2FjaGU=)

    下载并解压得到的 object-cache.php，上传到 wp-content 目录即可开启memcached缓存。

更新，2020年之后更好的办法是使用WPJAM-BASIC插件自带的object-cache.php,与WPJAM-BASIC插件配套使用。

详细方法是在安装WPJAM BASIC插件后，cd WPJAM-BASIC插件目录/template,找到object-cache.php

具体命令是

    cd /Wordpress-site-home-dir/wp-content/plugins/wpjam-basic/template/object-cache.php

Wordpress-site-home-dir替换成你wordpress的主目录。

将这个object-cache.php复制一份到以下目录

    Wordpress-site-home-dir/wp-content/

复制命令是

    cp -i object-cache.php /Wordpress-site-home-dir/wp-content/

推荐直接在宝塔面板里操作

值得说明的是，这里还有一个大坑等着你来踩：

> WordPress官网上的object-cache.php虽然也号称Memcached 插件，然而它只支持Memcache，不支持新版的，所以不能使用。如果错误地将object-cache.php和Memcached混用的话，则会出现WordPress打不开，前台后台页面一片空白的现象。

这也就是经常有站长反馈WordPress启用memcached功能后，页面空白的错误原因了。不巧，张戈在测试的时候也踩坑了，所以特别提出来，希望大家了解错误的原因，规避掉！

### 2、查看效果

做完第2步之后，你可以去网站前台刷新几次，产生缓存，然后从官方下载探针：

[http://pecl.php.net/get/memcache-3.0.8.tgz](https://zhang.ge/goto/aHR0cDovL3BlY2wucGhwLm5ldC9nZXQvbWVtY2FjaGUtMy4wLjgudGd6)

解压后，里面有一个memcache.php文件，编辑并找到如下代码：

    define('ADMIN_USERNAME','memcache');    // Admin Username
    define('ADMIN_PASSWORD','password');    // Admin Password
    define('DATE_FORMAT','Y/m/d H:i:s');
    define('GRAPH_SIZE',200);
    define('MAX_ITEM_DUMP',50);

    $MEMCACHE_SERVERS[] = 'mymemcache-server1:11211'; // add more as an array
    $MEMCACHE_SERVERS[] = 'mymemcache-server2:11211'; // add more as an array

修改如下：

    define('ADMIN_USERNAME','memcache');    // Admin Username 登录名称，自行修改
    define('ADMIN_PASSWORD','password');    // Admin Password 登录密码，自行修改
    define('DATE_FORMAT','Y/m/d H:i:s');
    define('GRAPH_SIZE',200);
    define('MAX_ITEM_DUMP',50);
    //下面是定义memcached服务器，一般我们是单机部署，所以注释掉一行，并将服务器地址根据实际修改，比如本文是127.0.0.1
    $MEMCACHE_SERVERS[] = '127.0.0.1:11211'; // add more as an array
    //$MEMCACHE_SERVERS[] = 'mymemcache-server2:11211'; // add more as an array

上传到网站私密目录（临时测试可以放到根目录），然后通过前台访问memcache.php这个文件，输入上面的用户名和密码即可看到memcached状态：

[![WordPress启用memcached动态缓存以及报错解决](mem2.png)](https://res.zgboke.com/wp-content/uploads/2016/05/mem2.png)

### 3、其他设置

如果发现页面可以打开，但是里面没有Hits数据，说明WordPress并没有成功连接到memcached，这时候我们可以在wp-config.php加入如下参数：

    global $memcached_servers;
    $memcached_servers = array(
        array(
            '127.0.0.1', // Memcached server IP address
             11211        // Memcached server port
        )
    );

实际的memcached监听IP和端口，你可以通过如下命令查看：

    netstat -nutlp | grep memcache

## WordPress清理Memcached缓存的方法

在[给服务器搬家后](https://blog.naibabiji.com/tutorial/wordpress-geng-huan-fu-wu-qi.html)一直还没有启动Memcached缓存，今天启动了一下，之前的缓存居然还没清除，所以只能自己手动来清理WordPress的Memcached缓存文件了。

网上搜的都是telnet的方法，在Linux服务器上如果没有安装telnet的话需要自己手动安装一次，CentOS安装telnet的命令如下：

    yum install telnet telnet-server

然后输入下面的命令就可以清理Memcached缓存信息了。

    telnet localhost 11211
    flush_all
    ctrl + ] //回车，输入这个后才能退出
    quit //退出

当然，还有另外一种更方便的方法，**直接在WordPress后台清理Memcached缓存**，那就是通过插件方式。

几年前开启Memcached缓存的站长朋友用的应该都是github上面的object-cache.php文件

而最近几年开启Memcached缓存的应该都是用的水煮鱼WPJAM插件自带的那个object-cache.php了。

也就是说，如果你装了水煮鱼的WPJAM插件，可以通过这个插件的系统信息界面去手动清除缓存。

![Memcached缓存清理](1558960685-memcached-compressor.png)

memcached插件有两个地方可以下载，我们可以访问github项目页面下载插件包：<https://github.com/tollmanz/wordpress-pecl-memcached-object-cache>
下载并解压得到的 object-cache.php，上传到 wp-content 目录即可开启memcached缓存。或者我们去wodpress后台的插件库里下载MemcacheD Is Your Friend，只要后台搜索下既可默认下载安装就可以了。

![img](6e7ef8e19a52f22787daacefe1da92a9dbc.jpg)

4、编辑wp-config.php 文件，上述所说步骤做完之后，编辑博客根目录的wp-config.php 文件，添加下方两段代码进去并保存：

```php
define('ENABLE_CACHE', true);
define('WP_CACHE', true);
```

![img](61217d2a9c2b3443dbe92fe7770beeedd07.jpg)

5、查看效果，检查是否配置成功，以及访问速度是否有提升，缓存命中率等等数据可以看得出了。

![img](3de74791ee596e1b7f76e0f14077be82628.jpg)
