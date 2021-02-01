---
title: 记录docker安装配置使用
siteurl: 记录docker安装配置使用
alias:
  - docker-wsl2/index.html
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 开发环境
tags:
  - 环境配置
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-5444'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-26T19:58:36+08:00'

---

[[docker-wsl2-hyper-v-proxifier-踩坑]]


## 安装 Docker

Docker在wsl2中支持两种安装方式，具体看下面的链接

<https://zhuanlan.zhihu.com/p/148511634>

一种是原生Linux的，即上面配置中所写的，另外一种是使用"Windows docker desktop" 通过使用内网的socks方式穿透，使得Linux成功挂载上安装在Windows上的docker，并且在Linux中使用docker命令

## 怎么查看docker安装在哪

- 在WSL2里面执行`df -Th`,会发现增加了一些新的与docker有关的挂载点。

WSL2下原生linux安装docker方式和完全linux虚拟机安装docker类似，区别在于WSL2下的linux不支持systemd。

Docker Desktop for windows方式，其实质是利用docker的C/S架构，

将windows模式下的docker对应docker.sock，docker客户端二进制和docker的数据目录挂载到WSL2里面的linux机器，

在此linux机器下执行docker命令(**docker命令为docker客户端**)，实质为客户端通过挂载的/var/run/docker.sock文件与windows里面的dockerd服务端进程通信。
如下图，我们在linux下重新启动linux下dockerd进程，linux模式下下载的busybox镜像又可以看到了，/var/run/docker.sock的时间戳也被更新了，

此时客户端通过/var/run/docker.sock文件与linux下的dockerd服务端通信。

3. 可以手动启动docker服务，输入命令：`service docker start`即可。

## 取消 service docker start 需要密码的机制

使用 service docker start 命令时, 需要使用 sudo 来执行. 而 sudo 需要输入密码. 阻碍了我们自动化开启 Docker.

下面使用 sudoers 文件配置使该命令不需要输入密码.

1. 创建 `/etc/sudoers.d/docker` 文件 (!注意文件不可包含符号 `.` 和符号 `~` ) 否则不会生效

```shell
sudo vi /etc/sudoers.d/docker
```

2. 将以下内容写入 `/etc/sudoers.d/docker`

```shell
# 用户($USER)在所有计算机(ALL)下可以使用root(root)的身份权限且不需要输入密码(NOPASSWD)来运行/usr/sbin/service(`which service`), 且限定参数为docker和一个任意参数(如:start,stop,status)
echo "$USER ALL=(root)  NOPASSWD: `which service` docker *" | sudo tee /etc/sudoers.d/docker
```

3. 根据 sudoers 的推荐设置, 修改 `/etc/sudoers.d/docker` 的权限,

```shell
sudo chmod 0440 /etc/sudoers.d/docker
```

### 配置 Docker 自启动

实现 WSL 下 Docker 自启动的方式有很多,下面介绍三种方式

1. 配置使用 systemd

   因为本身 WSL 没有使用 systemd 管理服务, 我也没有选择这种方式. 如需了解, 请参阅: <https://github.com/microsoft/WSL2-Linux-Kernel/issues/30>

2. 配置 `~/.bashrc` 或者 `/etc/profile` 等文件

2.1 直接添加启动代码到 `.bashrc` 中, 每次启动终端都会运行一次, 比较多余.

```shell
echo "sudo service docker start" | tee -a ~/.bashrc >/dev/null
```

2.2 直接添加启动代码到 `/etc/profile` 中, 启动 WSL 时运行一次, 稍显合理.

```shell
echo "sudo service docker start" | tee -a /etc/profile >/dev/null
```

2.3 保持 `/etc/profle` 无污染,加启动代码加到 `/etc/profile.d/docker.sh`中. bash 会在读取 `/etc/profile` 时自动加载 `profile.d` 下面的文件

```shell
# echo "service docker start" | tee -a ~/.bashrc >/dev/null
sudo touch /etc/profile.d/docker-start.sh && \
chmod 644 /etc/profile.d/docker-start.sh && \
echo "sudo service docker start" | tee -a /etc/profile.d/docker-start.sh >/dev/null
```

使用上面 3 种方式, 都可以使我们打开 WSL 时自动运行 Docker. 在开机之后, Docker 不会自动运行. 如果有服务依赖 Docker 的话, 我们仍需要手动打开 WSL. 建议使用下面介绍的方式.

3. 使用 Windows 的 `任务计划`

使用 Windows 的任务计划, 实现 Windows 开机之后自动执行 WSL 并使用命令启动 Docker 守护程序. 这样开机之后 Docker 便自动运行. 我们不需要再操心开启 Docker.

3.1 打开 `task scheduler`

![task-scheduler](task-scheduler.jpg)

3.2 创建新`task`

![create-task](create-task.jpg)

3.3 设置`Task`

![create-task-general](create-task-general.jpg)
![create-task-trigger](create-task-trigger.jpg)
![create-task-action](create-task-action.jpg)
![create-task-condition](create-task-condition.jpg)

## 配置docker 镜像代理

如果您遇到这种情况： 网络请求命令经常无法正常返回, 例如 `wget` 经常返回 4 状态, `curl` 也经常错误, 可以使用 `curl targetIP --max-time 3` 减少耗时, 

`docker build` 中，经常要拉取的一下开发工具也下载很慢或者无法下载, 这时候可以使用代理进行下载

在WSL2下，您的docke网络代理可以通过走Windows下的代理方式来实现，具体见[[类linux环境设置网络代理]]


## 配置 ENV 变量使用代理

```shell
ENV http_proxy "http://host.docker.internal:1080"
ENV HTTP_PROXY "http://host.docker.internal:1080"
ENV https_proxy "http://host.docker.internal:1080"
ENV HTTPS_PROXY "http://host.docker.internal:1080"
ENV ALL_PROXY "http://host.docker.internal:1080"
```

注意: **这里的 host.docker.internal 解析出来的是主机 IP, MacOS 中可以使用 docker.for.mac.localhost, Win 环境中使用 docker.for.win.localhost**

## 使用宿主网络

直接使用 `ENV` 命令固然能解决方法, 但是硬编码会导致各种环境需要相应修改, docker 中还有一种 host 网络模式, 让 container 使用宿主机的网络, 这里 `docker build --network host .` 也可以相应配置上

```shell
export http_proxy="http://host.docker.internal:1080"
export HTTP_PROXY="http://host.docker.internal:1080"
export https_proxy="http://host.docker.internal:1080"
export HTTPS_PROXY="http://host.docker.internal:1080"
export ALL_PROXY="http://host.docker.internal:1080"

docker build --network host .
```

- 转载修改自：<https://www.yzer.club/docker-proxy/>


## 两条常用docker命令

<https://github.com/Baiyuetribe/baiyue_onekey>

<https://baiyue.one/archives/1140.html>

停止的容器不会自动删除

我们可以使用 `docker ps -a` 查看所有的容器状态

```shell
[root@localhost ~]# docker ps -a
CONTAINER ID   ...  STATUS                       ...
e66458d65564   ...  Up 8 minutes                 ...
4558b3b54da0   ...  Exited (137) 31 minutes ago  ...
e08201b591cd   ...  Exited (0) 45 minutes ago    ...
6801e4604a32   ...  Exited (0) About an hour ago ...
```

那些 `STATUS` 栏以 `Exited` 开头的都是已经退出停止了的容器

我们可以使用下面的命令删除容器 `6801e4604a32`

已经停止的容器并不会自动删除，而是需要我们手动删除它们，这时候就要用到 `docker rm` 命令了

```shell
docker rm <container_id>
```

```shell
root@DESKTOP-VD0NEOU:~# docker ps -a
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS                    PORTS                      NAMES
7c5276bd917d        pch18/baota              "/bin/sh -c /entrypo…"   6 minutes ago       Created                                              baota
0413ec227379        pch18/baota              "/bin/sh -c /entrypo…"   9 minutes ago       Created                                              cool_pascal
9bb36caff4ac        pch18/baota              "/bin/sh -c /entrypo…"   14 minutes ago      Created                                              xenodochial_carson
a69e5ef43550        tobegit3hub/seagull      "./seagull"              23 minutes ago      Up 23 minutes             0.0.0.0:10086->10086/tcp   suspicious_boyd
b0e4bb08dadb        jrohy/v2ray              "./run.sh"               52 minutes ago      Up 52 minutes                                        v2ray
2f45c7a2c4f3        alpine/git               "git clone https://g…"   5 hours ago         Exited (0) 5 hours ago                               repo
7daf9f9bc912        hello-world              "/hello"                 6 days ago          Exited (0) 6 days ago                                kind_keldysh
a8da93bbf923        docker/getting-started   "/docker-entrypoint.…"   6 days ago          Exited (255) 6 days ago   0.0.0.0:80->80/tcp         nice_albattani
root@DESKTOP-VD0NEOU:~# docker rm 0413ec227379
0413ec227379
root@DESKTOP-VD0NEOU:~# docker rm 7c5276bd917d
7c5276bd917d
root@DESKTOP-VD0NEOU:~# docker rm 9bb36caff4ac
9bb36caff4ac
root@DESKTOP-VD0NEOU:~#
```

有一点需要注意，`docker rm` 命令只能删除已经停止的容器的 ID，还未停止的容器会报错

docker kill 容器id

<https://www.twle.cn/l/yufei/docker/docker-basic-container-rm.html>
