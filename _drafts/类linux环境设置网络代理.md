---
title: 类Linux环境设置网络代理
url: 类linux环境设置网络代理
alias:
  - WSL2设置网络代理/index.html
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-WSL2设置网络代理
date: '2021-01-25T21:50:08+08:00'
date updated: '2021-01-25T22:44:35+08:00'

---

## 普通Linux环境下的代理

在普通liunx环境下，127.0.0.1是指向主机物理地址的，这意味着你不需要什么繁琐的配置，

使用下面的命令

```shell
export HOST_IP=127.0.0.1
export HOST_PORT=1080
export PORXY_ADDR="http://${HOST_IP}:${HOST_PORT}"
```

## wsl2环境下的代理

wsl2下，我们一般通过访问 Windows 的网络代理应用方式来实现代理，于是就需要获取主机的物理IP

而wsl2环境下，物理ip是动态生成的, 每次重启后都可能不同，其本质上是你计算机的物理IP，随着路由器分配而变化

在WSL2环境下，我们有下面几种方式查看:

输入下面的命令获取：

```shell
ip route | grep default | awk '{print $3}'
```

WSL2 可以通过 `/etc/resolv.conf` 文件中的 nameserver 的值作为 IP 地址来访问 Windows 中的服务.

```shell
cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }'
```

下面给出shell的配置，实现自动获取IP用于代理

在你使用的shell 配置下，编写脚本动态获取物理IP

获取 nameserver 值 export 到环境变量 `HOST_IP` 中

加入代理地址环境变量(请替换成你的 Windows 本地代理端口)

shell 配置的其余内容最后会汇总，放到文章末尾

```shell
export HOST_IP=$(grep -oP '(?<=nameserver\ ).*' /etc/resolv.conf)
export HOST_PORT=1080
export PORXY_ADDR="http://${HOST_IP}:${HOST_PORT}"
```

有此地址便可方便地设置 `http_proxy` 等环境变量让 WSL 使用 Windows 下监听在 1080 端口的网络代理了

- 注意记得代理软件要开启允许局域网设备访问

## docker的代理设置

### docker的linux container和windows container

docker for windows 里面有切换linux container和windows container的选项，这两者有区别：

1. 在docker for windows中，windows container与物理主机公用一个kernel，windows container下，代理地址设置为`127.0.0.1`是正确的。

2. linux container则需要先运行一个linux 虚拟机，再基于虚拟机实现容器的应用。在linux container下因为隔了一层，代理地址当然不是`127.0.0.1`了，显然是要替换成物理主机的地址。

3. WSL2显然使用linux container的模式，于是在wsl2环境下,您首先需要获取物理主机的IP地址。

而获取主机物理地址的方式多种多样：

- 您可以选择使用docker提供的内置服务器，

- 使用上面提到的wsl2提供的获取IP服务，

- 使用docker官方推荐的渗透代理方式

- 注意：你的代理工具必须设置为允许来自局域网的连接

### 使用wsl2提供的IP获取服务

wsl2提供了获取物理IP的服务，

```shell
# CUSTOM CONFIG
export HOST_IP=$(grep -oP '(?<=nameserver\ ).*' /etc/resolv.conf)
# export HOST_IP=http://host.docker.internal
export HOST_PORT=1080
export PORXY_ADDR="http://${HOST_IP}:${HOST_PORT}"
```

然后进行下面的操作

```shell
vim /etc/default/docker
# 在/etc/default/docker中加入以下内容
export http_proxy="$PROXY_ADDR"
# 设置镜像
sudo mkdir -p /etc/docker && echo -e '{\n  "registry-mirrors": ["https://docker.mirrors.sjtug.sjtu.edu.cn"]\n}' | sudo tee /etc/docker/daemon.json
```

### 使用docker提供的内置服务器host.docker.internal

docker提供了一个内置服务器来帮助我们，将proxy设置为：

```shell
# CUSTOM CONFIG
# export HOST_IP=$(grep -oP '(?<=nameserver\ ).*' /etc/resolv.conf)
export HOST_IP=http://host.docker.internal
export HOST_PORT=1080
export PORXY_ADDR="http://${HOST_IP}:${HOST_PORT}"
```

'http://host.docker.internal' 会被映射成你真实主机的物理IP

然后进行下面的操作

```shell
vim /etc/default/docker
# 在/etc/default/docker中加入以下内容
export http_proxy="$PROXY_ADDR"
# 设置镜像
sudo mkdir -p /etc/docker && echo -e '{\n  "registry-mirrors": ["https://docker.mirrors.sjtug.sjtu.edu.cn"]\n}' | sudo tee /etc/docker/daemon.json
```

### 使用docker for desktop 提供的渗透代理设置

这是docker官方推荐的配置，而且你的配置很方便

[这里是官方的issue整合](https://github.com/microsoft/WSL/issues/4402)，解决这个专项问题

你只需要在docker for desktop 的设置中开启代理，代理地址填入

```shell
http://host.docker.internal:1080
```

我使用的是http隧道代理，所以用http协议开头，而我的代理端口是1080，根据你们的科学上网工具设置来填写

这样你的wsl2子系统下的docker就处于渗透代理模式了,效果等同于之前的几种方式

- 注意: **这里的 host.docker.internal 解析出来的是主机 IP, MacOS 中可以使用 docker.for.mac.localhost, Win 环境中使用 docker.for.win.localhost**

## docker内置服务器host.docker.internal的更多用法

前文说到，`host.docker.internal`是一个虚拟网络地址

原理上，Windows和MAC OS上使用`host.docker.internal`这个虚拟地址会自动解析出来你现在的物理ip地址(在笔者电脑中，是笔者正在使用的WLAN分配给笔者的IP V4 address)

那么你可以把这个地址用在wsl2、windows、mac os、Linux等一切需要获取物理地址的情况，前提是你的docker需要保持后台运行

例如下面这种

```shell
# HTTP [git]
git config --global http.https://github.com.proxy http://host.docker.internal:1080
```

`host.docker.internal` 会解析出来你现在的物理ip地址，在使用效果上等同于你在Windows上使用的127.0.0.1

- localhost和127.0.0.1并不总是相等的，只是大部分主机上，他们的值是相同的

- WSL2下的localhost并不能解析成你的主机物理地址，所有需要使用这种方式

- 关于localhost、物理主机地址、127.0.0.1之间的联系，具体可以看这篇文章，<https://www.jianshu.com/p/ad7cd1d5be45>

## 附最终脚本和配置(Github)

以下是笔者开关代理的脚本, 在终端使用 proxy / unproxy 命令即可 开启/关闭 代理 :

```shell
# CUSTOM MIRRORS 在下面3种方式中，选择一种方式取消注释
# 普通Linux环境
# export HOST_IP=127.0.0.1
# wsl2的IP获取服务
# export HOST_IP=$(grep -oP '(?<=nameserver\ ).*' /etc/resolv.conf)
# 使用docker提供的内置服务器host.docker.internal
# export HOST_IP=http://host.docker.internal
export HOST_PORT=1080
export PORXY_ADDR="http://${HOST_IP}:${HOST_PORT}"

# PROXY FOR APT
function proxy_apt() {
    # make sure apt.conf existed
    sudo touch /etc/apt/apt.conf
    sudo sed -i '/^Acquire::https\?::Proxy/d' /etc/apt/apt.conf
    # add proxy
    echo -e "Acquire::http::Proxy \"$PORXY_ADDR\";\nAcquire::https::Proxy \"$PORXY_ADDR\";" | sudo tee -a /etc/apt/apt.conf >/dev/null
    echo "current apt proxy status: using $PORXY_ADDR, proxying"
}

function unproxy_apt() {
    sudo sed -i '/^Acquire::https\?::Proxy/d' /etc/apt/apt.conf
    echo "current apt proxy status: direct connect, not proxying"
}

# PROXY FOR NPM
function proxy_yarn() {
    yarn config set proxy $PORXY_ADDR
    yarn config set https-proxy $PORXY_ADDR
    npm config set proxy $PORXY_ADDR
    npm config set https-proxy $PORXY_ADDR
}

function unproxy_yarn() {
    yarn config delete proxy
    yarn config delete https-proxy
    npm config delete proxy
    npm config delete https-proxy
}

function proxy_npm() {
    proxy_yarn
}

function unproxy_npm() {
    unproxy_yarn
}

# if docker is not running, then start docker service
# if docker running, then `:`, else `sudo service docker start`
# The `:` Do nothing beyond expanding arguments and performing redirections. The return status is zero.
# https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html#Bourne-Shell-Builtins
ps -C dockerd 1>/dev/null && : || sudo service docker start 1>/dev/null

# PROXY FOR DOCKER
function proxy_docker() {
    # make sure /etc/default/docker existed
    sudo touch /etc/default/docker
    sudo sed -i '/^export https\?_proxy/Id' /etc/default/docker
    # add proxy
    echo -e "export http_proxy=\"$PORXY_ADDR\";
export https_proxy=\"$PORXY_ADDR\";
export HTTP_PROXY=\"$PORXY_ADDR\";
export HTTPS_PROXY=\"$PORXY_ADDR\";" | sudo tee -a /etc/default/docker >/dev/null &&
        sudo service docker restart &&
        echo "current docker proxy status: using $PORXY_ADDR, proxying"
}

function unproxy_docker() {
    # sudo sed -i '/^Acquire::https\?::Proxy/d' /etc/apt/apt.conf
    # 删除代理设置
    sudo sed -i '/^export https\?_proxy/Id' /etc/default/docker &&
        sudo service docker restart &&
        echo "current docker proxy status: direct connect, not proxying"
}

# GLOBAL PROXY FOR BASH

# if ~/.bash_proxy not exist , then create it.
if [ ! -f ~/.bash_proxy ]; then
    touch ~/.bash_proxy
fi
# execute ~/.bash_proxy
. ~/.bash_proxy

function proxy() {
    echo -e "export {all_proxy,http_proxy,https_proxy,ALL_PROXY,HTTP_PROXY,HTTPS_PROXY}=\"$PORXY_ADDR\";" | tee ~/.bash_proxy >/dev/null
    # apply
    . ~/.bash_proxy
    # declare
    echo "current proxy status: using $PORXY_ADDR, proxying"
}

function unproxy() {
    # unset all_proxy http_proxy https_proxy ALL_PROXY HTTP_PROXY HTTPS_PROXY
    echo -e "unset all_proxy http_proxy https_proxy ALL_PROXY HTTP_PROXY HTTPS_PROXY" | tee ~/.bash_proxy >/dev/null
    # apply
    . ~/.bash_proxy
    # declare
    echo "current proxy status:  direct connect, not proxying"
}
```

> 参阅: <https://docs.microsoft.com/en-us/windows/wsl/wsl2-ux-changes#accessing-windows-applications-from-linux>

## Git-Bash设置代理

Git在Windows下，使用了Windows的mingw64等环境,模拟实现了Linux shell的大部分功能，但是其本质上还是和我们通常所说的类Linux环境有很大区别

具体代理设置见：[[git-代理配置方案]]
