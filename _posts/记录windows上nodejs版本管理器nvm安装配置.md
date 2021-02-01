---
title: 记录Windows上nodejs版本管理器nvm安装配置
siteurl: 记录windows上nodejs版本管理器nvm安装配置
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-ea92'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-27T20:09:11+08:00'

---

[nvm最新的下载地址](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fcoreybutler%2Fnvm-windows%2Freleases)

Node版本管理器--nvm，可以运行在多种操作系统上。nvm for windows 是使用go语言编写的软件。 我电脑使用的是Windows操作系统，所以我要记录下在此操作系统上nvm的安装和使用。

## 下载

nvm-windows 最新下载地址：
[https://github.com/coreybutler/nvm-windows/releases](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fcoreybutler%2Fnvm-windows%2Freleases)
如图所示：

我目前看到有两个版本【Pre-release 1.1.6】和 【Latest release 1.1.5]，我们下载目前稳定版本1.1.5就可以了。1.1.6版本是最新版本，可能还不是很稳定。
而这里又有四个可下载的文件。

- nvm-noinstall.zip： 这个是绿色免安装版本，但是使用之前需要配置
- nvm-setup.zip：这是一个安装包，下载之后点击安装，无需配置就可以使用，方便。
- Source code(zip)：zip压缩的源码
- Sourc code(tar.gz)：tar.gz的源码，一般用于*nix系统

我对这个目前只是简单使用，为了方便，所以下载了nvm-set.zip文件。

## 安装和升级

### 安装之前的操作

**请注意**： 在安装nvm for windows之前，你需要卸载任何现有版本的node.js。并且需要删除现有的nodejs安装目录（例如："C:\Program Files\nodejs’）。因为，nvm生成的symlink（符号链接/超链接)不会覆盖现有的（甚至是空的）安装目录。
你还需要删除现有的npm安装位置（例如“C:\Users\weiqinl\AppData\Roaming\npm”），以便正确使用nvm安装位置。

### 安装

以上操作完成之后，双击执行下载的setup文件，

图2：双击之后的界面

Next之后，选择同意协议，之后选择nvm的本地安装目录，**这里注意，nvm的安装路径名称中最好不要有空格。**

例如最好不要这样有空格的`~\Program Files\nvm`，我这里选择的是`D:\softtool\nvm`。
点击Next，跳转到设置 Node.js的Symlink，即需要设置nodejs的快捷方式存放的目录。

图4：nodejs安装的目录

之后，点击Next-->Install-->Finish完成本次安装。

### 检测

检查是否安装成功，我们可以在新的命令窗口中输入

```undefined
nvm
```

- 如果出现nvm版本号和一系列帮助指令，则说明nvm安装成功。
- 否则，可能会提示`nvm: command not found`

图5：nvm安装成功检测

### 升级

如果要升级的话，请重新[下载最新的安装程序](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fcoreybutler%2Fnvm-windows%2Freleases)。并直接运行安装程序。它将安全的覆盖需要更新的文件，而无需关心nodejs的安装。
此次安装需要确保和上次使用相同的安装目录。
如果你最初安装到默认位置，则只需一直点击"下一步"，直到完成。

## 使用

nvm for windows是一个命令行工具，在控制台输入`nvm`,就可以看到它的命令用法。基本命令有：

- `nvm arch [32|64]` ： 显示node是运行在32位还是64位模式。指定32或64来覆盖默认体系结构。
- `nvm install <version> [arch]`： 该 `<version>` 可以是node.js版本或最新稳定版本`latest`。（可选[arch]）指定安装32位或64位版本（默认为系统arch）。设置[arch]为`all`以安装32和64位版本。在命令后面添加`--insecure` ，可以绕过远端下载服务器的SSL验证。
- `nvm list [available]`： 列出已经安装的node.js版本。可选的available，显示可下载版本的部分列表。这个命令可以简写为`nvm ls [available]`。
- `nvm on`： 启用node.js版本管理。
- `nvm off`： 禁用node.js版本管理(不卸载任何东西)
- `nvm proxy [url]`： 设置用于下载的代理。留`[url]`空白，以查看当前的代理。设置`[url]`为`none`删除代理。
- `nvm node_mirror [url]`：设置node镜像，默认为`https://nodejs.org/dist/.`。我建议设置为淘宝的镜像*[https://npm.taobao.org/mirrors/node/](https://link.jianshu.com/?t=https%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Fnode%2F)*
- `nvm npm_mirror [url]`：设置npm镜像，默认为`https://github.com/npm/npm/archive/`。我建议设置为淘宝的镜像*[https://npm.taobao.org/mirrors/npm/](https://link.jianshu.com/?t=https%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Fnpm%2F)*
- `nvm uninstall <version>`： 卸载指定版本的nodejs。
- `nvm use [version] [arch]`： 切换到使用指定的nodejs版本。可以指定32/64位[arch]。`nvm use <arch>`将继续使用所选版本，但根据提供的值切换到32/64位模式的`<arch>`
- `nvm root [path]`： 设置 nvm 存储node.js不同版本的目录 ,如果`<path>`未设置，将使用当前目录。
- `nvm version`： 显示当前运行的nvm版本，可以简写为`nvm v`

一个nodejs的安装使用流程：

```rust
nvm ls   // 查看目前已经安装的版本
nvm install 6.10.0  // 安装指定的版本的nodejs
nvm use 6.10.0  // 使用指定版本的nodejs
```

这是我安装第一个版本时候的命令:

图6：这是我安装第一个版本时候的命令

认真看以下的图，相同的`nvm ls`命令，得到的结果为什么不一样？因为，这是使用了nvm切换到了指定的版本。如果在`nvm ls`命令输出了 当前样式，说明切换成功了。如果没有出现`(Currently using 64-bit executable)`，则表示没有切换成功。这就需要查看原因，认真按照上面步骤来。

图7：nvm ls展示已经安装的nodejs版本

### 使用命令时注意点

- 请用管理员身份运行命令管理器，否则可能出错。
- 先设置[node](https://link.jianshu.com/?t=https%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Fnode%2F)和[npm](https://link.jianshu.com/?t=https%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Fnpm%2F)的淘宝镜像，这样成功率和下载速度会更高点。

## 用途

1：主要用途，切换nodejs版本。如果想使用最新的流行版本测试您正在开发的模块，而不用卸载稳定版本的node，则可以使用nvm来切换nodejs版本。

## 注意点

- nvm安装目录，最好不要存在空格。否则，nvm可以安装成功，但使用nvm use x.y.z（nodejs的切换）会有问题。
- 有些全局的npm模块，可能在各版本的node.js之间不共享。
  你正在使用的node.js版本中可能不支持某些npm模块。因此在工作的时候请注意工作环境。

博客园地址：[http://www.cnblogs.com/weiqinl/](https://link.jianshu.com/?t=http%3A%2F%2Fwww.cnblogs.com%2Fweiqinl%2F)
