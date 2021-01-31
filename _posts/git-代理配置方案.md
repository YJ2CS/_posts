---
title: Git 代理配置方案
url: git-代理配置方案
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - Git
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-b807
date: 2020-11-16 17:54:00
---

[#技巧](https://wiki.imalan.cn/tag/技巧/)

国内连 Github 是个很头疼的问题。浏览器还好，只要电脑挂了代理一般就没什么问题；主要是终端使用有些麻烦。折腾出来的最佳实践如下。

部分来自：<https://molunerfinn.com/git-ssh2https/>

## 设置代理

首先，代码仓库尽量使用 HTTPS 协议，也就是以 `https://github.com` 开头的链接，而不是以 `git@github.com` 开头的
SSH 链接，这样就可以通过 `git config` 方便地设置代理：

```shell
git config --global http.proxy socks5://127.0.0.1:8080
```

网上广泛流传的针对 `https.proxy` 也设置一遍代理是不必要的。

注意，这样设置后 push 时可能会提示

```shell
ServicePointManager 不支持具有 socks5 方案的代理
```

但是仍然能 push 成功。最好的解决方法是在本地使用 HTTP/HTTPS 代理，例如：

```shell
git config --global http.proxy http://127.0.0.1:1081
```

具体的端口视你电脑本地的代理软件（Shadowsocks、Clash等）而异。

## 设置仓库

以上设置只针对使用 HTTPS 协议的仓库有效，如果已经使用了 SSH 协议，则可以如此修改：

```shell
git remote set-url origin https://xxxxx #your https repo url
```

查看是否修改成功：

```shell
git remote -v
origin  https://github.com/xxx/xxx.github.io.git (fetch)
origin  https://github.com/xxx/xxx.github.io.git (push)
```

## 设置 Git

使用 HTTPS 协议后 push 时需要密码，我们可以将密码保存起来，免得重复输入。编辑或新建文件：

```shell
vi ~/.git-credentials
```

添加一行：

```shell
https://用户名:密码@github.com
```

保存退出，然后执行：

```shell
git config --global credential.helper store
```

即可。

### 提升安全性

以上可见用户名与密码是明文存储在配置文件中的，如果你觉得不够安全可以继续下面的步骤。

对于 Mac 而言，可以将 git 默认的 `credential.helper` 指定成 `osxkeychain`

```
git config --global credential.helper osxkeychain
```

然后可以通过

```
$ git config credential.helper
osxkeychain
```

来查看是否指定成功了。之后用户名密码将会保存在系统自带的 `keychain access` 里，也就是通过 macOS 的钥匙链管理密码。

### Windows

使用微软开发的[Git-Credential-Manager-for-Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows)

最新版下载地址：<https://github.com/Microsoft/Git-Credential-Manager-for-Windows/releases/latest>

安装之后，在控制台里输入：

```
git config --global credential.helper manager
```

之后还是一样的，某个项目里输入用户名密码之后，以后就再也不用输入了。你可以在`管理网络密码`里找到你的用户密码——其实不是密码，而是 token 了，因为 `Git-Credential-Manager-for-Windows` 它会自动调用 Github 的 API 生成 `Personal access tokens`，你可以在你的 Github 的 `Personal settings 找到它。

`管理网络密码` -> 控制面板\用户帐户\凭据管理器\windows凭据

转载自

<https://wiki.imalan.cn/archives/Git%20%E4%BB%A3%E7%90%86%E9%85%8D%E7%BD%AE%E6%96%B9%E6%A1%88/>

## 分辨需要设置的代理

- HTTP 形式：

  > git clone <https://github.com/owner/git.git>

- SSH 形式：

  > git clone [git@github.com](mailto:git@github.com):owner/git.git

## 一、HTTP 形式

### 走 HTTP 代理

```shell
git config --global http.proxy "http://127.0.0.1:18080"
git config --global https.proxy "http://127.0.0.1:18080"
```

### 走 socks5 代理（如 Shadowsocks）

```shell
git config --global http.proxy "socks5://127.0.0.1:1080"
git config --global https.proxy "socks5://127.0.0.1:1080"
```

### 取消设置

```shell
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 二、SSH 形式

linux中
修改 `~/.ssh/config` 文件（不存在则新建）：

```shell
# 必须是 github.com
Host github.com
   HostName github.com
   User git
   # 走 HTTP 代理
   # ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=8080
   # 走 socks5 代理（如 Shadowsocks）
   # ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
```

对于Windows用户，要使用socks5代理却没有 nc 的，可以将
ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
换成
ProxyCommand connect -S 127.0.0.1:1080 %h %p

遇到这个问题的，都是配置不正确。

更新：GitHub把Windows上的socks5端口访问ssh给关了，

显示远程主机已关闭连接，建议Windows上用https吧
1、检查端口。
2、检查依赖命令是否存在

```shell
Host *
  ServerAliveInterval 120
  ServerAliveCountMax 5

# macOS 给 Git(Github) 设置代理（HTTP/SSH）
# https://gist.github.com/chuyik/02d0d37a49edc162546441092efae6a1
# 0x00 克隆 repo 的两种方式：https 和 ssh 方式
# https 方式：git clone https://github.com/owner/git.git
# ssh   方式：git clone     git@github.com:owner/git.git

# 0x01 https 方式克隆的 repo，走 http 或 sock5 代理，任选一个
# 0x0101 http 代理
# git config --global http.proxy "http://127.0.0.1:1087"
# git config --global https.proxy "http://127.0.0.1:1087"
# 0x0102 sock5 代理
# git config --global http.proxy "socks5://127.0.0.1:1086"
# git config --global https.proxy "socks5://127.0.0.1:1086"
# 0x0103 取消使用代理
# git config --global --unset http.proxy
# git config --global --unset https.proxy

# 0x02 ssh 克隆方式的代理设置，直接在全局设置文件配置，即 ~/.ssh/config 文件
Host github.com
   HostName github.com
   User git
   # 走 HTTP 代理，需要 brew install socat
   # ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=1087
   # 走 socks5 代理（如 Shadowsocks）
   ProxyCommand nc -v -x 127.0.0.1:1086 %h %p
   # 走 socks5 代理（如 Shadowsocks），Windows 平台没有 nc 命令
   # ProxyCommand connect -S 127.0.0.1:1086 %h %p
```
