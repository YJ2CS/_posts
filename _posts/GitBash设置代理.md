---
title: GitBash设置代理
siteurl: GitBash设置代理
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
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-GitBash设置代理
date: 2021-01-02T18:36:29.000Z
date updated: '2021-01-03T14:05:53+08:00'

---

> Do not let what you cannot do interfere with what you can do.
> — <cite>John Wooden</cite>

## git Bash的安装目录（Git安装目录）

git Bash的安装目录（Git安装目录）对应的是

```bash
cd path/to/install/Git/
```

在我的电脑上就是

```bash
cd 'C:\Program Files\Git'
```

而在git bash中虚拟以后,就映射到了 '/'

```bash
# path/to/install/Git/ <==> 'C:\Program Files\Git' <==> '/'
```

即您可以直接使用命令

```bash
cd /
```

## gitBash的配置文件

gitBash有许多用于不同功能的配置文件，具体见[lhajh.github.io](https://lhajh.github.io/windows/git/2019/05/05/Window-platform-Git-Bash-configuration.html)

而在本文中，我们只用到设置代理相关的文件

`host.docker.internal`是dockers内置的一个虚拟地址，它映射到了你的主机真实IP，而`7890`是你科学上网软件定义的端口

```bash
vim ~/.bashrc
```

文件末尾中添加

```bash
# CUSTOM MIRRORS

# `host.docker.internal`是dockers内置的一个虚拟地址，它映射到了你的主机真实IP，
# 而`7890`是你科学上网软件定义的端口
export HOST_IP=host.docker.internal
export HOST_PORT=7890
export PORXY_ADDR="http://$HOST_IP:$HOST_PORT"

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

然后在

```bash
vim ~/.bash_profile

```

文件末尾中添加

```bash
# Loading .bashrc File

source ~/.bashrc
```
