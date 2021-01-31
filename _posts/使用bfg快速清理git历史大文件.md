---
title: 使用bfg快速清理git历史大文件
url: 使用bfg快速清理git历史大文件
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 开发环境
tags:
  - 环境配置
  - 转载
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-b7e
date: 2020-11-10 22:16:00
---
- 转载自： [使用bfg快速清理git历史大文件_紫月7575的博客-CSDN博客 - https://blog.csdn.net/](https://blog.csdn.net/qq_36254947/article/details/108641438)


[[清理git存储库]]

[toc]
## 背景

> 之前写过一篇的,使用的git命令清理的大文件,但是我3G多的git,.git文件夹里面的pack就3G多,而且是个好几年并且在持续开发的项目,里面的提交成千上万了,每次使用`git filter-branch`,都要好几个小时，我研究了一下，要彻底清理项目中的那一堆大文件，只要要用脚本连续跑两天。。。
> 最近发现了一个方案，使用bfg，我仅仅十几分钟就处理完了
> 原先的方案：<https://blog.csdn.net/qq_36254947/article/details/108601940>

- 下载jar包:<https://rtyley.github.io/bfg-repo-cleaner/#download>
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200917141445172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MjU0OTQ3,size_16,color_FFFFFF,t_70#pic_center)

## 步骤

- 解除保护分支
  默认情况下,git项目是有一个保护分支的
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914084933518.png#pic_center)

![image-20200911181110097](https://img-blog.csdnimg.cn/2020091714235168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MjU0OTQ3,size_16,color_FFFFFF,t_70#pic_center)

- 拉取代码

> 注意:需要ssh拉取,http不行(ssh拉取需要配置ssh密钥)
> [git配置ssh密钥](https://blog.csdn.net/tao5375/article/details/81938210)

```shell
git clone --mirror git项目的ssh地址
# 拉取的是 项目名.git  文件夹,这是Git项目中的.git文件夹
12
```

- 查看大文件

```shell
# 进入项目文件夹
cd xxx.git
# 查询大文件
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -10 | awk '{print$1}')"
# tail -n 代表查看前n个大文件,
12345
```

- 清理文件夹

```shell
java -jar bfg-1.13.0.jar --delete-folders 清理的文件夹名字  本地git项目地址
# 注意:文件夹名字,而不是文件夹路径,要小心别把其他文件夹中的同名的文件夹删除了
# 若是jar包放到了git项目中,不需要添加本地git项目地址 ,但最好不要放到.git文件夹中
123
```

- 清理文件

```shell
java -jar bfg-1.13.0.jar --delete-files 删除的文件名 本地git项目地址
# 注意:这是文件名只是名字部分,不包含路径
12
```

- 清理无效文件

```shell
# 删除的文件和文件夹,需要这一步才会真正清除
git reflog expire --expire=now --all && git gc --prune=now --aggressive
12
```

- 查看大小

```shell
git count-objects -vH
# 此时就能发现项目小了
12
```

- 推送

```shell
git push -f  # -f 强制推送
1
```

## 脚本

```bash
# 开始
time1=$(date)
echo 开始时间 $time1

echo 拉取项目
git clone --mirror git@1xxxx.git
jarpach="D:\test1\bfg-1.13.0.jar"  # jar包地址

cd "D:\转换\test1\xxx.git"  # git项目地址

git count-objects -vH
echo
echo
echo
echo 清理文件提交
echo
# 将需要清理的文件列出来
java -jar $jarpach --delete-folders 文件夹1  
java -jar $jarpach --delete-folders 文件夹2
java -jar $jarpach --delete-files dist.*
java -jar $jarpach --delete-files 文件2

echo
echo

echo 清理无效文件
echo git reflog expire --expire=now --all && git gc --prune=now --aggressive
echo
echo
echo 清理完成

git count-objects -vH

echo
time2=$(date)
echo 开始时间 $time1 ---结束时间 $time2

12345678910111213141516171819202122232425262728293031323334353637
```

清理完之后,若是没有问题,进入项目文件夹使用`git push -f`强制推送即可,之后别忘记清理服务器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200917143837950.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200917143909774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MjU0OTQ3,size_16,color_FFFFFF,t_70#pic_center)

## 清理服务器

清理完文件之后,需要清理一下服务器

```shell
# 从git服务器进入这个项目的.git文件夹
cd /var/opt/gitlab/git-data/repositories/xx/.git  #根据配置可能不同
# 清理文件
git gc --prune=now --aggressive
#查看大小
git count-objects -vH
123456
```

> 注意:我清理完之后,git服务器使用http不能拉取代码了,重启服务器解决了
>
> - [git clone异常 【fatal: protocol error: bad line length character: Inte】](https://blog.csdn.net/qq_36254947/article/details/108641179)

## 笔记

```bash
# 清理文件夹
# 注意:文件夹名字,而不是文件夹路径,要小心别把其他文件夹中的同名的文件夹删除了
# 若是jar包放到了git项目中,不需要添加本地git项目地址 ,但最好不要放到.git文件夹中
java -jar bfg-1.13.0.jar --delete-folders .\\themes\\next

# 删除的文件和文件夹,需要这一步才会真正清除
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```
