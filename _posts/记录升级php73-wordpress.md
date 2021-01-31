---
title: 记录升级PHP73_WordPress
url: 记录升级php73_wordpress
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-5062'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T20:08:26+08:00'

---

环境 阿里云 linux apache mode下的PHP

## [升级脚本](https://developer.aliyun.com/article/717769)

```bash
#!/bin/bash
WORKDIR=/tmp/
PHP73_DIR=/usr/local/php73
DEFAULT_SWAP=0

createSwap(){
  if [ `cat /proc/meminfo | grep SwapTotal | awk -F   '{print $2}'` -ne 0 ]
  then
    return 0
  fi
  if [ `cat /proc/meminfo | grep MemTotal | awk -F   '{print $2}'` -le 1048576 ]
  then
    echo Mem lower than 1GB,creating swap...
    dd if=/dev/zero of=/swap bs=1M count=2048
    mkswap -f /swap
    swapon /swap && echo SWAP create success.
    DEFAULT_SWAP=1
  fi
}

installDependence(){
  yum install -y libxml2-devel openssl-devel  curl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel xslt libxslt-devel
  yum update -y curl curl-devel
  yum remove -y libzip
}

compileLibzip(){
  echo start install libzip.
  cd ${WORKDIR}
  if [ -f libzip-1.2.0.tar.gz ]
  then
    rm -rf libzip-1.2.0.tar.gz
  fi
  wget https://code.aliyun.com/yh11/download/raw/master/libzip-1.2.0.tar.gz
  tar -zxvf libzip-1.2.0.tar.gz
  cd libzip-1.2.0
  ./configure
  make && make install
  if [ $? -ne 0 ]
  then
    echo libzip install failed.
    exit 127
  fi
  ln -s /usr/local/lib/libzip/include/zipconf.h /usr/local/include
}

installPHP(){
  echo Install PHP 7.3
  cd ${WORKDIR}
  if [ -f php-7.3.9.tar.gz ]
  then
    rm -rf php-7.3.9.tar.gz
  fi
  wget https://code.aliyun.com/yh11/download/raw/master/php-7.3.9.tar.gz
  tar -xvf php-7.3.9.tar.gz
  cd php-7.3.9
  ./configure --prefix=/usr/local/php73  --enable-soap --enable-cgi --with-mysql=/usr/local/mysql --with-mysqli=mysqlnd --with-gd --with-pdo-mysql=mysqlnd --with-zlib --enable-zip --enable-fpm --without-pear --disable-phar --with-openssl --enable-mbstring=all --with-jpeg-dir=/usr --with-png-dir=/usr --with-curl --with-freetype-dir=/usr --enable-gd-native-ttf --with-xsl=/usr --enable-calendar --enable-exif --enable-ftp --with-iconv --enable-bcmath --with-mcrypt=/usr/local/libmcrypt --enable-opcache
  make && make install
  if [ $? -ne 0 ]
  then
    echo PHP install failed.
    exit 127
  fi
  cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm73
  cp ${PHP73_DIR}/etc/php-fpm.conf.default ${PHP73_DIR}/etc/php-fpm.conf
  chmod +x /etc/init.d/php-fpm73
}

createConfig(){

cat << EOF > ${PHP73_DIR}/etc/php-fpm.d/www.conf
[www]
listen = /home/www/logs/php73-fpm.sock
listen.mode = 0666
user = www
group = www
pm = dynamic
pm.max_children = 128
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 15
pm.max_requests = 300
rlimit_files = 1024
slowlog = /home/www/logs/php73-fpm-slow.log
EOF

}

modifyApache(){
  sed -i 's#ProxyPassMatch ^/(.*\.php(/.*)?)$ unix:/home/www/logs/php-fpm.sock|fcgi://127.0.0.1/home/www/htdocs#ProxyPassMatch ^/(.*\\.php(/.*)?)$ unix:/home/www/logs/php73-fpm.sock|fcgi://127.0.0.1/home/www/htdocs#g' /usr/local/apache/conf/httpd.conf
}

restartService(){
  chkconfig php-fpm off
  chkconfig php-fpm73 on
  /etc/init.d/php-fpm stop
  /etc/init.d/php-fpm73 start
  /etc/init.d/apachectl restart
}

delSwap(){
  if [ ${DEFAULT_SWAP} -eq 1 ]
  then
    swapoff /swap
    rm -rf /swap
  fi
}

createSwap
installDependence
compileLibzip
installPHP
createConfig
modifyApache
restartService
delSwap

```

## 怎么执行sh脚本

[](https://blog.csdn.net/magi1201/article/details/75194515)<https://blog.csdn.net/magi1201/article/details/75194515>

1。新建一个sh文件

vim [setup.sh](http://setup.sh)

内容是文章中的。

1. 给权限

chmod 764 [setup.sh](http://setup.sh)

3.在当前目录使用命令

./setup.sh

### [查找php.ini](http://www.findme.wang/blog/detail/id/278.html)

说到查找，当然首先想到的是find命令。执行如下命令，即可查找到php.ini文件

```shell
sudo find / -name php.ini 
```

![](21eb06fd.png)
可是，找到三个php.ini文件，具体哪个是当前正在运行的PHP使用的配置文件呢？ PHP提供了两种方式，可供使用。

方法一

这个是比较简单的方法，使用如下命令，可以清楚的看出当前的php使用的配置文件。

```shell
php --ini 
```

![](5e2c4808.png)
方法二

打印出phpinfo()，然后就可以看出了，如下：
阿里云网站首页为

cd /home/www/htdocs/

vim phpinfo.php

内容为

```php
<?php
phpinfo();
?>
```

![记录升级](d6ee58a1.png)

上文脚本指定的位置在
cd /usr/local/php73/lib

针对这个方法，也可以在命令行中查看，有如下两种方式：

php -i |grep php.ini //php -i其实就是输出phpinfo

php -r phpinfo(); |grep php.ini

![记录升级](4f037097.png)

本文为原创文章，请尊重辛勤劳动，如需转载，请保留本文地址 [](http://www.findme.wang/blog/detail/id/278.html)<http://www.findme.wang/blog/detail/id/278.html>

chmod 764 setup.sh

## [修改WordPress中上传附件2M大小限制的方法](https://blog.csdn.net/u010486124/article/details/38348327?depth_1-)

搜索一下几个关键字：

memory_limit、post_max_size、upload_max_filesize、max_execution_time、max_input_time

### 解释选项

file_uploads = on ;是否允许通过HTTP上传文件的开关。默认为ON即是开 upload_tmp_dir = /tmp/ ;文件上传至服务器上存储临时文件的地方，如果没指定就会用系统默认的临时文件夹/tmp/

一般默认的设置值为：

（注意下，这些设置不是在一起的，是分开的，需要自己查找修正）

memory_limit=8M ;每个PHP页面所吃掉的最大内存，默认8M//相当于单个脚本可调用内存大小 post_max_size=8M ;指通过表单POST给PHP的所能接收的最大值，包括表单里的所有值。默认为8M//上传文件大小上限 upload_max_filesize=2M ;允许上传文件大小的最大值。默认为2M//默认上传文件大小，这个就是2M的限制！ max_execution_time=30 ;每个PHP页面运行的最大时间值(秒)，默认30秒//最大执行时间，页面等待时间 max_input_time=60 ;每个PHP页面接收数据所需的最大时间，默认60秒

如果要上传>8M的大体积文件，需要进一步配置以下参数

### 进一步配置以下的参数

把上述参数修改后，在网络所允许的正常情况下，就可以上传大体积文件了 max_execution_time = 600 max_input_time = 600 memory_limit = 128m upload_max_filesize = 128m post_max_size = 128m

## [重启apache服务器](https://segmentfault.com/q/1010000006748362)

阿里云中

## 停止旧版本的PHP(实际不停止也不影响，停止可以减少一些系统资源占用)

/etc/init.d/php-fpm stop

## 启动新版PHP-FPM

/etc/init.d/php-fpm73 start

## 重启apache

/etc/init.d/apachectl restart

比较常见的：service apache2 restart

假设你apahce安装目录为/usr/local/apache2 apahce启动命令：/usr/local/apache2/bin/apachectl start apaceh apache停止命令：/usr/local/apache2/bin/apachectl stop apache重启命令：/usr/local/apache2/bin/apachectl restart 重启时不中断当前的连接，则应：/usr/local/sbin/apachectl graceful

如果apache是linux服务 service httpd start 启动 service httpd restart 重新启动 service httpd stop 停止服务

Ubuntu系统 启动 # sudo /etc/init.d/apache2 start 停止 # sudo /etc/init.d/apache2 stop 重启 # sudo /etc/init.d/apache2 restart

更新，直接看我的另外一篇文章

记录wordpress迁移网站，从阿里云到阿里云

里面记录了我使用宝塔面板作为Linux上的服务器运维工具，并因此备份wordpress，重装liunx，恢复备份的全过程

即推荐直接使用宝塔作为运维工具实现对PHP73的升级，同时解决阿里云上默认的应用镜像Wordpress环境变量配置不全的问题，这个问题十分折磨人，直接导致你有许多服务器的运维工具，运维命令无法使用，包括最基础的php 命令，都得想方设法绕开，使用变通方法。
