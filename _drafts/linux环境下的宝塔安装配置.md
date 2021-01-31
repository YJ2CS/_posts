---
title: linux环境下的宝塔安装配置
url: linux环境下的宝塔安装配置
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-linux环境下的宝塔安装配置
date: '2021-01-24T16:40:58+08:00'
date updated: '2021-01-25T21:55:20+08:00'

---

> All courses of action are risky, so prudence is not in avoiding danger (it's impossible), but calculating risk and acting decisively. Make mistakes of ambition and not mistakes of sloth. Develop the strength to do bold things, not the strength to suffer.
> — <cite>Niccolo Machiavelli</cite>

安装步骤跟服务器安装一样，没任何区别。ubuntu下的安装命令如下：

wget -O install.sh <http://download.bt.cn/install/install-ubuntu_6.0.sh> && sudo bash install.sh
运行后，等待安装完成即可，此过程可以不需要梯子，直接安装。

## 关于wsl2里部署宝塔

根据宝塔官方说法，从未测试在子系统中部署宝塔（2020-06-14），同时建议宝塔部署在服务器上，wsl2环境下出现问题实属正常，因为没有适配

> 见<https://www.bt.cn/bbs/thread-50950-1-1.html>

经过测试，在WSL 1中可以正常安装，WSL 2中无法顺利安装，可能是WSL 2是虚拟后的独立Linux系统，与WSL 1不同

官方答复是未测试在WSL子系统内部署面板，建议使用服务器系统环境使用搭建

- 本地wsl环境还是直接用docker吧

不过，有些特殊需求情况下，我们需要在本地环境做一些模拟服务器环境的测试，本地的优势是超低延迟，带来的极速反馈。具体表现在网页刷新、响应、vs-code里代码的保存和执行上，都是毫无卡顿。

wsl2真就这么方便吗？其实也不一定，熟悉桌面版docker的人而言，安装docker版宝塔其实也是非常不错的，唯一的缺陷就是Windows桌面版docker环境启动需要1分钟左右，而wsl2的启动时间，几乎为0。如果要重置环境，wsl2可以直接卸载ubantu就可以了，而桌面版docker只需要rm一下就行。当然你也可以在wsl2上运行docker环境，使用宝塔版docker。也是不错的体验。

目前不论是wsl2还是桌面版docker，其实都会占用本地空间，而且这个空间是只增不减的，比如你安装的docker镜像，就算命令行卸载了，磁盘占用空间是不会缩减的。桌面版docker环境，需要重置下才能恢复。而wsl的只能卸载重装了。卸载也就几秒，安装也就几秒，反正你想怎么折腾就怎么来。

## 一键脚本

25+优质开源项目，任意搭建，任意二开（需搭配vs remote）

```shell
bash <(curl -L -s https://raw.githubusercontent.com/Baiyuetribe/baiyue_onekey/master/go.sh)
```

![image-20201119024751692](image-20201119024751692.png)

安装宝塔docker版会失败，失败后如何删除看上面的，建议不要使用这个docker脚本
