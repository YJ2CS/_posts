---
title: wordpress使用fastcgi-cache-优化提速
url: wordpress使用fastcgi-cache-优化提速
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-wordpress使用fastcgi-cache-优化提速'
date: 2020-12-18 22:43:35
---
[[WordPress插件选择]]

## [WordPress]WordPress 用上 Nginx fastcgi_cache 缓存教程，给博客再提速（更新于：20191123）

-  [程序相关](https://24dian30.com/category/manage/program)

一直想折腾一下缓存方面的东西，但是一直没有动手去做。一是因为自己技术不到家，二也是因为折腾稍微有点麻烦，我人一懒下来就什么也不想折腾了。这次的话是因为偶然看到感兴趣了，下了个决心把这个 fastcgi_cache 弄好，结果也没花多少时间，其中大多数时间还都花在了后面的调试上。虽然是弄好了，实际能有多大的提速效果还不太清楚，不过毕竟是弄了静态缓存，总不会比动态生成网页要慢了，再加上静态之后也不用每次请求都丢 PHP 那边了，机器负载方面应该能轻松一些，总的来说这波不亏。

关于这个 fastcgi_cache 的作用，我就不多谈了，对于 WordPress 来说就是把平常都需要动态生成的网页进行缓存，访客来访的时候可以不再去请求动态网页，直接返回已经缓存好的静态网页，达到加快网站的相应速度和减轻服务器负载的效果。这么说当然也不是信口开河，毕竟动态网页的生成既要时间也要使用服务器资源，而缓存为静态网页之后就没那么多事了，不需要经过计算生成，直接把文件丢过来就了事。当然，没有缓存的时候还是需要先动态生成一次的。

话虽这么说，使用 fastcgi_cache 一个不好的地方就是它只负责缓存，并不关心缓存之后的网页有没有更新内容什么的，也就是说缓存之后的网页除非你特别设置，否则是会一直保持不变的。为了解决这个问题，我们需要借助其它工具来与它配合，在一定条件下及时删除已有的缓存并重新生成，以及对某些特定的地方不生成缓存。

好了话不多说，开始说说我是怎么折腾的吧！

## 自己的折腾环境

虽然教程是适合大部分人的，不过为了以防万一，我还是需要交代一下自己的折腾环境，好让大家能根据自己的实际情况，对下面的教程进行微调。

系统：Debian

环境：LNMP 一键安装包搭建，Nginx 版本为 1.15.8

WordPress 版本：5.0.3

虽然我是用 LNMP 安装的 Nginx，不过大家放心，本教程绝大部分内容都是通用的，无非是在配置方面稍有差别，我能想到的地方都会和大家说明的。

## 步骤一：安装 ngx_cache_purge 模块（本人已弃用可不安装）

ngx_cache_purge 这个模块是我们必须安装的，所以先把它装好再进行 fastcgi_cache 的配置。

上面那句话是我原本写的，这里给大家详细交代一下什么情况下才需要安装这个模块，为什么安装这个模块，以及我为什么弃用了这个模块。

要说起这个，首先我们得说说 Nginx 的 FastCGI Cache 这个模块了。FastCGI Cache 作为一个轻量级的缓存模块，用起来还是没什么大问题的，但是很可惜它有一些限制，其中最突出的就是没有一个很好的删除缓存的方法。FastCGI Cache 模块默认只能根据你设置的缓存时间来自动清理缓存，没有一个很好的办法来手动清理缓存，要想指定额外的缓存清理方法，需要用他们的商业版才可以。

为了解决这个问题，网友们想出来了两种方法：一是通过安装额外的模块，也即是这里说的 ngx_cache_purge 来让我们有办法手动清理缓存；二则是通过某些代码，直接物理删除缓存文件，来让 Nginx 重新生成缓存。

这两种方法我只试用过方法二，方法一为什么不试用呢，这也是我弃用这个模块的原因——不知道是模块久未更新还是因为插件的原因，方法一在我这边没有效果。又因为我目前只找到一个能支持方法一清除缓存的插件，在没有替代品的情况下，只得放弃方法一用方法二了。

但是，方法二也不是完美的。方法二确实会删除缓存文件，但是这种删除方式是很暴力的。Nginx 不仅仅只是单纯的生成缓存文件，也会存储缓存文件的一些信息比如在缓存文件多久无人访问时进行删除，我们用方法二删除了缓存文件，但是这些信息还是存储在 Nginx 中的，它会在设定好的条件下继续尝试删除缓存文件，然后因为缓存文件已经被我们暴力的删除了，便会产生报错信息，告知我们删除失败，文件已经不存在了。虽然这不会对我们的体验造成太大的影响，但是能不报错自然不报错的好，后面说配置时我会告知大家我的解决办法，当然暂时还未验证。

好了给大家交代清楚了，因为目前方法一我还没找到很好的解决办法让它能实际用起来，所以我本人选择了弃用。如果以后这种方法我测试成功了，或找到其它支持这种方法的插件能让它正常工作，我会再跟大家说的，就目前来看，我们只能选择方法二清除缓存，因此请跳过步骤一。

下面的安装方法我还是予以保留，省得以后解决了我还要重新写：

注：以下安装教程全部在命令行环境下进行，只说一次。

1.下载解压 ngx_cache_purge 模块
```bash
cd /root/src

wget http://labs.frickle.com/files/ngx_cache_purge-2.3.tar.gz

tar zxvf ngx_cache_purge-2.3.tar.gz
```
这里说明一下，代码第一句演示的是选择模块的解压存放目录，要存在别的地方就自己改路径，我就不罗嗦了。

如果你用的不是 LNMP 安装的 Nginx，步骤一接下来的编译 Nginx 教程请不要观看，可以在网上找找正常的 Nginx 是怎么编译进新模块的。这部分的操作都不难，我不单独写出来只是因为分两个来讲对我来说会比较麻烦。

2.编辑 LNMP 配置文件，加入模块的编译参数

怎么编辑就不用我说了吧？LNMP 的配置文件名为 lnmp.comf，位于 LNMP 的安装目录下，自己找找然后用 vi 或者别的什么东西，在配置文件相应位置加上代码就 OK 了：
```conf
Nginx_Modules_Options='XXXXXX --add-module=/root/src/ngx_cache_purge-2.3'
```
注：这里写 XXXXXX 是因为考虑到有人也编译了其它的模块，所以写出来代替一下别的模块的编译参数，就是提醒大家别的东西不需要改，加最后那一句上去就行了。另外，主义最后一句写的文件夹路径，要和前面下载保存 ngx_cache_purge 的路径对应。

3.使用升级脚本重新编译一遍 Nginx，使得新模块能编译进 Nginx

参考官方的升级教程：https://lnmp.org/faq/lnmp1-2-upgrade.html

简单举例如下（假设 LNMP 安装在 /root/lnmp1.5 目录下）：
```bash
cd /root/lnmp1.5

./upgrade.sh nginx
```
看到提示之后输入你想升级的 Nginx 版本号，只是想编译的话输入现在用的 Nginx 版本号就是了。

## 步骤二：编辑网站的 Nginx 配置文件 

这一步非常重要，根据 Nginx 配置文件结构的不同可能会稍有差异，我简单的说一下。

我们要加的配置主要分为两部分，一个部分是需要加在 Nginx 配置文件的 http 块里面的，另一部分是加在我们自己网站配置的 sever 块里面的。

以一个比较简单 Nginx 配置文件结构为例，它大概是这样子的：

#前略
```conf
http

    {

        XXXXXX

sever

    {

        XXXXXX

    }

}
```
如果我们将我们要加入的 http 块的代码用 A 来代替，写进 sever 块的代码用 B 来代替，那么配置文件应该这么改：

#前略
```conf
http

    {

        XXXXXX

        A

sever

    {

        XXXXXX

        B

    }

}
```
这么说大家能理解了吗？下面我再来说说具体的比如 LNMP 的 Nginx 配置文件结构，大概长这样：

#前略
```
http

    {

        XXXXXX

sever

    {

        XXXXXX

    }

include vhost/*.comf;

}
```
这里多了一句 include vhost/*.comf; 也就是额外包含了某个位于 Nginx 安装目录 vhost 文件夹下的 conf 配置文件，vhost 下的文件是我们配置虚拟主机对应的网站用到的配置文件，它的结构是这样子的：
```conf
sever

    {

        XXXXXX

    }
```

所以说，针对 LNMP 装的 Nginx 配置文件的结构，我们可以把 A B 都写在 vhost 的配置文件里，再把 vhost 配置文件和前面的 Nginx 配置文件结合，实际上的效果是这样子的：

#前略
```conf
http

    {

        XXXXXX

sever

    {

        XXXXXX

    }

#include  vhost/*.comf;的实际效果如下

A

sever

    {

        XXXXXX

        B

    }

}
```
不知道大家理解了我的意思没有？总之我是想说，如果你的配置文件结构是像最开始说的那种，全部内容都写在 nginx.comf 里的，那么你直接改那个文件就可以了。而如果是像 LNMP 这种包含了虚拟主机配置文件的情况，我们的配置只需要加在 vhost 目录下对应网站的配置文件里就行了。

说了一堆有的没的很抱歉，下面还是直接以 LNMP 安装的 Nginx 为例来编辑配置文件吧，当然正如我前面说的，我们直接编辑 vhost 这个目录下的对应配置文件就行了，比如 
```conf
/usr/local/nginx/conf/vhost/example.com.comf：

fastcgi_cache_path /tmp/wpcache levels=1:2 keys_zone=WORDPRESS:250m inactive=2d max_size=1G use_temp_path=off;

fastcgi_cache_key "$scheme$request_method$host$request_uri";

fastcgi_cache_use_stale error timeout invalid_header updating http_500;

fastcgi_ignore_headers Cache-Control Expires Set-Cookie;

fastcgi_cache_lock on;

fastcgi_cache_background_update on;

server

    {

        #前略，原本有的东西不需要改动

  # nginx  fastcgi cache 缓存设置

  set $skip_cache 0;

        # post 访问不缓存

        if ($request_method = POST) {

            set $skip_cache 1;

        }   

        #动态查询不缓存

        if ($query_string != "") {

            set $skip_cache 1;

        }   

        #后台等特定页面不缓存（其他需求请自行添加即可）

        if ($request_uri ~* "/wp-admin/|/xmlrpc.php|wp-.*.php|/feed|index.php|sitemap(_index)?.xml") {

            set $skip_cache 1;

        }   

        #对登录用户不展示缓存

        if ($http_cookie ~* "conment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logged_in") {

            set $skip_cache 1;

        }

    

  #继续上面的内容

  location ~ [^/]\.php(/|$)

            {

    fastcgi_pass  unix:/tmp/php-cgi.sock;

    fastcgi_index index.php;

    include fastcgi.comf;

    include pathinfo.comf;

                #新增的缓存规则

                fastcgi_cache_bypass $skip_cache;

                fastcgi_no_cache $skip_cache;

                add_header X-Cache "$upstream_cache_status Fron $host";

    add_header Cache-Control "no-cache, must-revalidate, max-age=0";

    add_header X-Frame-Options SAMEORIGIN;

    add_header X-XSS-Protection "1; mode=block";

                fastcgi_cache WORDPRESS;

                fastcgi_cache_valid 200 301 302 1d;

           }

       #缓存清理配置（可选模块，请细看下文说明）

       location ~ /purge(/.*) {

            allow 127.0.0.1;

            allow "此处填写你服务器的真实外网IP";

            deny all;

            fastcgi_cache_purge WORDPRESS "$scheme$request_method$host$1";

        }

    }
```
上面是我的配置，代码部分的参考：

张戈博客：[Nginx开启fastcgi_cache缓存加速，支持html伪静态页面](https://zhangge.net/5042.html)

挖站否：[WordPress开启Nginx fastcgi_cache缓存加速方法-Nginx配置实例](https://wzfou.com/nginx-fastcgi-cache)

easyengine：[fastcgi_cache with conditional purging](https://easyengine.io/wordpress-nginx/tutorials/single-site/fastcgi-cache-with-purging)

对上面代码的解读我会分三个部分来说明，大家可以根据自己的情况来修改。

首先是：
```conf
fastcgi_cache_path /tmp/wpcache levels=1:2 keys_zone=WORDPRESS:250m inactive=2d max_size=1G use_temp_path=off;

fastcgi_cache_key "$scheme$request_method$host$request_uri";

fastcgi_cache_use_stale error timeout invalid_header updating http_500;

fastcgi_ignore_headers Cache-Control Expires Set-Cookie;

fastcgi_cache_lock on;

fastcgi_cache_background_update on;
```
这一部分是常规配置下需要加在 http 块里的内容，我只挑几个需要注意的地方和大家说明一下：

**fastcgi_cache_path**

设置缓存文件的存放目录，缓存空间名称和大小。这里我设置的存放目录是 /tmp/wpcache（此目录需要你事先创建好，并设置好文件夹的所有者和组，LNMP 的设置为 www:www），空间名称（key_zone）为 WORDPRESS，缓存最大空间（max_size）为 1 G。缓存目录只建议设到两级，也就是像我一样的 /tmp/wpcache 这样，因为据张戈博客所说的，设三级会引起后面安装的插件的某个 bug。另外需要说的是，如果你和我一样选择将缓存目录设置在 /tmp 下，那么要注意这个目录下的文件会在机器重启的时候自动清除。

这里有几个需要注意的地方：

1. keys_zone=WORDPRESS:250m 这里的 250m 是指的存储缓存信息的空间最大容量，不是缓存目录的最大容量，根据官方的说法，1m 大概能存储八千条信息（你把信息理解成网址就行了=_=）。考虑到当空间占满可能会触发 Nginx 的自动删除机制然后出现报错信息（通过插件我们自己把缓存删除了，Nginx 找不到要删除的缓存就会报错，实际问题不是很大，只是看着不爽），建议设置大一些。
2. inactive=2d 指的是当缓存长期没人使用到达一定时间时（我这里设置的是 2 天），不管这个缓存到没到期，都进行删除。注意，因为我们用插件自己删除了缓存导致当触发这个机制时 Nginx 可能会因为要删除的缓存不存在而产生报错信息，所以我们要想想办法关闭这个机制。目前想到的办法是将这里设置的时间大于 fastcgi_cache_valid 缓存更新的时间（我设置的是 1 天），只触发缓存更新机制（产生新缓存覆盖）而不是物理删除机制。此想法未经验证，请大家自行判断是否有效。
3. use_temp_path=off 的作用是让在生成缓存产生的临时文件也存储在缓存目录中，生成成功之后直接重命名即可变为缓存。关于为什么这么设置，也是有原因的，根据官方的说法，如果有单独指定临时文件的存储目录与缓存目录，且他们不在同一个文件系统上时，因为会有一个跨文件系统的复制操作，可能会有一些性能上的损失，所以官方比较推荐将缓存目录与临时文件的存储目录设置在同一文件系统中，这里为了方便，我就不再去调整存储目录之类的了，直接选择关闭了这项功能。

PS：通过设置个性化的缓存文件存放目录，可以达到将缓存存放在硬盘或内存上的效果，这里就不多说了。

**fastcgi_cache_use_stale**

规定在和 fastcgi 服务器通信的过程中遇到哪种错误时可以使用过时的缓存。这里其实不需要怎么改动，我自己多加了一个 updating，作用是在缓存更新还未完成的时候，先用旧的缓存来代替，这样子可以减轻缓存更新期间 PHP 那边的压力（配合后面的 fastcgi_cache_background_update on; 风味更佳）。

**fastcgi_ignore_headers**

忽略某些缓存控制头，简单理解为扩大咱们能生成的缓存文件的范围就好了，一般不需要改动。

**fastcgi_cache_lock on**

简单说就是给缓存加个锁，一次只允许一个新缓存生成请求，其它相同的请求需要等待这个请求的完成，在同时有大量缓存需要更新的时候可以减轻服务器的压力。默认情况下，开启了锁之后请求有 5s 的时间限制，超过 5s 缓存还没生成，就让别的请求进来（这些请求不会产生缓存）。

**fastcgi_cache_background_update on**

允许产生一个子请求来更新已经过期的缓存，配合上面 fastcgi_cache_use_stale 地方的 updating 设置使用。

接下来说 sever 块这边的配置，首先看代码：
```
server

    {

        #前略，原本有的东西不需要改动

  # nginx  fastcgi cache 缓存设置

  set $skip_cache 0;

        # post 访问不缓存

        if ($request_method = POST) {

            set $skip_cache 1;

        }   

        #动态查询不缓存

        if ($query_string != "") {

            set $skip_cache 1;

        }   

        #后台等特定页面不缓存（其他需求请自行添加即可）

        if ($request_uri ~* "/wp-admin/|/xmlrpc.php|wp-.*.php|/feed|index.php|sitemap(_index)?.xml") {

            set $skip_cache 1;

        }   

        #对登录用户不展示缓存

        if ($http_cookie ~* "conment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logged_in") {

            set $skip_cache 1;

        }

    
```
  #继续上面的内容
```conf
  location ~ [^/]\.php(/|$)

            {

    fastcgi_pass  unix:/tmp/php-cgi.sock;

    fastcgi_index index.php;

    include fastcgi.comf;

    include pathinfo.comf;

                #新增的缓存规则

                fastcgi_cache_bypass $skip_cache;

                fastcgi_no_cache $skip_cache;

                add_header X-Cache "$upstream_cache_status Fron $host";

    add_header Cache-Control "no-cache, must-revalidate, max-age=0";

    add_header X-Frame-Options SAMEORIGIN;

    add_header X-XSS-Protection "1; mode=block";

                fastcgi_cache WORDPRESS;

                fastcgi_cache_valid 200 301 302 1d;

        }

    }
```
前面的缓存设置部分配置代码中已经有所说明了，应该不难理解吧？所以关于哪些东西不缓存这部分，我就只挑几个重要的说了。

关于后台页面不缓存和登录用户不缓存这个，原本大家提供的代码是这样子的（借用张戈博客的代码来说明下）：

#后台等特定页面不缓存（其他需求请自行添加即可）
```conf
if ($request_uri ~* "/wp-admin/|/xmlrpc.php|wp-.*.php|/feed|index.php|sitemap(_index)?.xml") {

    set $skip_cache 1;

}   
```
#对登录用户、评论过的用户不展示缓存（这个规则张戈博客并没有使用，所有人看到的都是缓存）
```conf
if ($http_cookie ~* "conment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logged_in") {

    set $skip_cache 1;

}
```
对比之后发现，我删除了两处：/feed/和 conment_author。/feed/这里原本的意思是不缓存订阅源页面的，但是我觉得这个页面缓存了也没有关系，一是因为订阅源除非发布新文章，否则平常都是不会更新的，不缓存太可惜了；二是通过后面我们安装的插件的更新缓存功能，完全可以做到在发了新文章的时候同时清除/feed/的缓存，这样子就不会有订阅不更新的问题了。

此处的补充说明：上面代码中，我已经将/feed/重新加回来了，并不是说这么设置有什么问题，而是因为一些插件方面的原因，导致设置缓存 Feed 之后没有很妥当的办法在有新文章发布时更新它，只能根据 Nginx 设置的时间 1 天更新一次，所以不得已暂时决定不缓存 Feed 了。

至于 conment_author，原本的意思是对评论过的用户，不展示缓存。但是我觉得这个不合理，因为后面安装的插件的关系，在有新评论产生时，会同步删除缓存，所以评论过的人看到的页面刷新后也是能看到新评论的，不需要额外放行。当然这里有一种情况，就是对评论表单做了某种样式的修改，可以记住评论者的状态（也就是二次评论会自定帮你写好用户名邮箱什么的）。这种情况下还是针对评论用户放行的好，不然后面看到这个页面的人可能会发现想评论的时候，因为缓存了前面那个人的评论状态，导致他这里会自动写上缓存的那个人的用户名和邮箱信息，给人带来困扰。根据张戈博客主所说，要解决这个问题，可以将评论自动填写表单功能做成 Cookie 的形式。

此处的补充说明：由于现有的主题 Dobby 并不具备此种功能，暂时还是加回来了，如果你的评论测试没问题，可以考虑我前面说的把 conment_author 这个删除了。

说完这些之后，再来说说后面这一部分的代码：

**继续上面的内容**
```conf
location ~ [^/]\.php(/|$)

    {

  fastcgi_pass  unix:/tmp/php-cgi.sock;

  fastcgi_index index.php;

  include fastcgi.comf;

  include pathinfo.comf;

        #新增的缓存规则

        fastcgi_cache_bypass $skip_cache;

        fastcgi_no_cache $skip_cache;

        add_header X-Cache "$upstream_cache_status Fron $host";

  add_header Cache-Control "no-cache, must-revalidate, max-age=0";

  add_header X-Frame-Options SAMEORIGIN;

  add_header X-XSS-Protection "1; mode=block";

        fastcgi_cache WORDPRESS;

        fastcgi_cache_valid 200 301 302 1d;

        }
```
先说说“#新增的缓存规则”前面的那部分内容。这一部分内容实际上是取自于 LNMP 装的 Nginx 中的 enable-php-pathinfo.comf 这个文件里的代码，主要是针对动态网站的一些设置。在我们没有改的配置文件里，本身应该能找到类似 include enable-php-pathinfo.comf; 这样的代码。那为什么我们还要特地把这个文件里的代码添加进去呢？主要是因为 location 块的某种匹配机制问题，碰到正则匹配时，先匹配到的就不会再往后进行匹配了。也就是说如果我们不特别加上这个，那么尽管后面有针对 location ~ [^/]\.php(/|$) 的配置，它也不会合并在同一个块里生效，而是只会生效我们“新增的缓存规则”，造成网站的错误。

此外，如果你有其它针对 PHP 的正则配置之类的（比如禁止某个目录 PHP 文件的访问权限什么的），建议将本配置放在那些配置之后，因为 location 的正则匹配是从上往下匹配的，匹配到了一个就不会再继续往下进行匹配了。或者你也可以想办法将两个 location 块合并在一起表示。

PS：此处为自己经历到错误时想到的，不一定都是正确的，欢迎大佬指正。

前面的说完了，再说说我加的那些规则吧，还是老样子，挑重点的说。

**新增规则部分，一二的规则不要乱动**

**add_header X-Cache “$upstream_cache_status Fron $host”;**

会在网站返回的静态网页缓存中添加 X-Cache 响应头，用来展示缓存的命中情况。比如：HIT Fron 24dian30.com 这样子，告诉你命中了缓存，现在返回的是缓存网页。不想加域名的话，去掉 Fron $host 就行了，或者你也可以来个独特的写法，看你个人喜欢。

**add_header Cache-Control**

设置缓存网页的浏览器缓存控制。这部分想自己设置的可以去网上找找 Cache-Control 这个头的有关说明，我这里的设置简单的说就是静态网页在用户的浏览器这边不做缓存，每次都需要到服务器获取最新的静态网页缓存。

PS：这个头是很有必要加上的，不然可能会导致浏览器不知道本地的缓存页面该怎么处理。我在测试时就发现，没有加缓存控制头时，Edge 浏览器会在命中缓存之后，点什么页面链接都会弹出下载保存对话框。这个问题我没有深究，还望知道的人能跟我分享下经验。

**五六条的配置主要是针对网页安全方面的，看个人情况可加可不加**

**fastcgi_cache WORDPRESS;**

缓存的空间名称，看最开始设置的 key_zone。

**fastcgi_cache_valid 200 301 302 1d;**

对返回哪种状态码的网页进行缓存。200 301 302 都是状态码，不懂的可以去查找下有关知识，这里一般不需要改动，最后的 1d 表示缓存默认会保存一天。

第三部分要说的就是针对前面我说的，方法一用模块来让 Nginx 支持特定条件的缓存清除功能的配置了：

#缓存清理配置（可选模块，请细看下文说明）
```conf
        location ~ /purge(/.*) {

            allow 127.0.0.1;

            allow "此处填写你服务器的真实外网IP";

            deny all;

            fastcgi_cache_purge WORDPRESS "$scheme$request_method$host$1";

        }
```
正如配置里的说明，加不加这个 location 块全看你是不是用的方法一清除的缓存，如果时按照我文里说的做的，就没必要加。这一部分也没有什么好说明的，因为模块一是通过请求来让 Nginx 清除缓存的，为了防止别有用心的人随便清除缓存，所以前面设置了两个 allow，意思是只有本地（127.0.0.1）和服务器 IP 发来的清除缓存请求才会被允许，其他人的则全部拒绝（deny all）。

关于配置文件的说明基本就说到这里了，其实关于 FastCGI Cache 的配置，还有很多更为详细的配置项，大家如果感兴趣可以去看看官方的文档：https://nginx.org/en/docs/http/ngx_http_fastcgi_module.html

搞定了上面的配置之后，别忘了重载一下 Nginx，比如 LNMP 的重载命令如下：
```bash
lnmp nginx reload
```
上面做完之后，其实现在你应该就启用好缓存了，随便找一个页面刷新一下（注意要用未登录未保存 Cookie 的状态访问哦），看看页面 HTML 的响应头，如果出现了如下图的东西，并且状态是 HIT，就代表缓存成功并返回给你了。

![img](https://24dian30.com/wp-content/uploads/2019/01/2019011901.png)

## 步骤三：安装 WordPress 插件并进行设置

上面我们已经做好了 fastcgi 的缓存配置，到这里的时候你就会发现它已经开始正常工作了，但是这个时候我们还没有一种很好的方法对生成的缓存进行自动清除，所以我们需要选择安装一个 WordPress 插件，它可以帮助我们在有新评论或者新文章产生时，自定帮我们清理对应的网页缓存，这样子就解决了缓存长久不更新的问题了。

安装方法我就不说了，在 WordPress 后台的插件中心搜索并安装启用就可以了，我主要还是给大家说说有哪些插件可以选择以及怎么设置的问题。

### 插件一：Nginx Helper

关于 Nginx Helper 这个插件，目前在我这边测试没有办法正常工作，因此建议大家选择别的，如果这个插件能用的话，应该是 WordPress 插件里最好的一个了。它支持我在步骤一中说的两种清除方法，但是我测试都不行。

20191123 更新：不能使用的问题已经解决，博主犯了个常识性错误……大家可以正常使用了。

前面的设置不要乱改，根据我写的来。关于 Purge Method 这项，请看我在步骤一里的有关说明，如果你用的方法一就选择第一种（别忘了后面的配置部分要加上最后说的那个可选的 location 块），如果是方法二就选择第二种。

![img](https://24dian30.com/wp-content/uploads/2019/01/011601.png)

后面一部分的设置根据自己的来更改，设置完了记得保存设置生效：

![img11](https://24dian30.com/wp-content/uploads/2019/01/011602.png)

这里特别说一下最后的 Custon Purge URL，前面我说我写配置时选择将 /feed/ 订阅源也给缓存了，但是为了让订阅源也能及时刷新，所以这里就得加上它，让他能随着新文章的发布而及时更新缓存。

这里的格式是一行一个链接，不需要带上我们的域名。比如我要清除缓存的链接是 https://24dian30.com/feed/，那我在这里只需要写上 /feed/ 就可以了。

另外这里需要补充说明一点，**很重要所以大家务必要看**！

前面我的代码演示说明里已经说了，我将 fastcgi 的缓存保存目录设置为了 /tmp/wpcache，但是 Nginx Helper 默认能识别的目录是 /var/run/nginx-cache，如果我们写的目录和默认目录不同，会导致这个插件没办法正常的工作。

为了解决这个问题，最后还需要我们改一下 WordPress 根目录下的 wp-config.php 文件，在其中新增以下代码（不知道写什么位置好的就写在文件最前面吧）：

define( 'RT_WP_NGINX_HELPER_CACHE_PATH','/tmp/wpcache');

将其中的/tmp/wpcache 改成你自己设置的目录就行了。

以下内容来自 20191123 的更新，希望对大家能有所帮助：非常感谢垃圾君⑨在自己博客写的经验分享，文章在这里：《[试用 nginx Fastcgi cache月余感想](https://51acg.eu.org/试用-nginx-fastcgi-cache月余感想.html)》 虽然内容不多不过确实是让我醒悟过来到底自己是哪里做错了。

在萌茶我近期的测试中，我发现虽然 Nginx Helper 有在正常工作，但是不管怎么使用上面的常量定义，Nginx Helper 始终只认自己的目录 /var/run/nginx-cache，然后在偶然的机遇下看到了上面说的那篇文章，总算是明白了过来为什么。

wp-config.php 这个文件中包含有 WordPress 预先设置好的一些常量定义，其中非常重要的是在文件末尾写的这两个：
```php
 if ( !defined('ABSPATH') )

  define('ABSPATH', dirname(__FILE__) . '/');

require_once(ABSPATH . 'wp-settings.php');
```

我们在编辑 wp-config.php 文件时，应该也有看到注释提示在这停止编辑，而我一时疏忽将 Nginx Helper 的常量定义写在了这个文件的末尾也就是上面这两个的后面，导致了常量定义实际没有生效，才造成了一系列的错误。

因此 Nginx Helper 看起来并不存在什么问题，只要大家别像我一样把常量定义写在文件末尾就行了。

### 插件二：Cache Sniper for Nginx

这是本人曾今使用的插件，支持的是方法二的物理删除缓存文件的方法，虽然相比起前面的这位功能不是很丰富，但是设置简单，功能也勉强够用。

![img](https://24dian30.com/wp-content/uploads/2019/01/2019011801.png)

插件用起来没什么问题，清除全部缓存的功能和自动清除的功能都能很好的工作。只是没办法像 Nginx Helper 那样清除归档类页面的缓存这点比较可惜。

### 其它插件

可以用来清除 FastCGI cache 的插件其实 WordPress 插件中心里的也不是很多，所以我就不一一说了。目前我还测试了一个插件，叫 Nginx Cache，与前两者相比它只支持在有内容变动时简单的全部删除缓存文件（方法二），没办法单独清理缓存，看作者的问题回答貌似也不打算做复杂的样子，没有复杂需求的朋友也可以试试。

## 结束

写了这么多，总算是写完了，不知道对大家有没有帮助？对网站提速不是随便说说做做就能搞定的，需要考虑到很多方面，希望大家能明白。不过，即使 fastcgi cache 没法让你的网站提速，对减轻服务器在高并发时的压力也是能有一定帮助的，折腾一下也无妨。

**声明:** 本文采用 [BY-NC-SA](https://creativeconmons.org/licenses/by-nc-sa/4.0/) 协议进行授权，如无注明均为原创，转载请注明转自 [24点半](https://24dian30.com/)
本文地址: [[WordPress\]WordPress 用上 Nginx fastcgi_cache 缓存教程，给博客再提速（更新于：20191123）](https://24dian30.com/manage/program/2748.html)

