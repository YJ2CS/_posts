---
title: git配置使用ssh
siteurl: git配置使用ssh
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - Git
tags:
  - Git
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-e02c'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T11:56:42+08:00'

---

[toc]

## git配置SSH

配置SSH可以解决IDEA提交代码到github上时出现Push failed:Could not read from remote repository.错误

从2021年起，github不再允许http以及https链接。所以我们需要配置git的ssh链接方式。

原文链接：https://blog.csdn.net/u013778905/article/details/83501204

```text
https://github.com/xiangshuo1992/preload.git

git@github.com:xiangshuo1992/preload.git
```

这两个地址展示的是同一个项目，但是这两个地址之间有什么联系呢？

前者是https url 直接有效网址打开，但是用户每次通过git提交的时候都要输入用户名和密码，有没有简单的一点的办法，一次配置，永久使用呢？当然，所以有了第二种地址，也就是SSH URL，那如何配置就是本文要分享的内容。

GitHub配置SSH Key，这在Linux上能够帮助我们在通过git提交代码时，不需要繁琐的验证过程，简化操作流程。

同时这也是我们必须要做的，因为不再允许http和https的连接方式。

[GitHub上关于这部分的官方文档](https://docs.github.com/cn/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh)

## 设置git的user name和email

如果你是第一次使用，或者还没有配置过的话需要操作一下命令，自行替换相应字段。

```config
git config --global user.name "YJ2CS"

git config --global user.email  "cnyjzhang@outlook.com"
```

说明：git config --list 查看当前Git环境所有配置，还可以配置一些命令别名之类的。

## 检查现有 SSH 密钥（Windows中）

在生成 SSH 密钥之前，您可以检查是否有任何现有的 SSH 密钥。

[GitHub上关于这部分的官方文档](https://docs.github.com/cn/free-pro-team@latest/github/authenticating-to-github/checking-for-existing-ssh-keys)

打开 Git Bash。

输入 ` ls -al ~/.ssh  `以查看是否存在现有 SSH 密钥：

```bash
$ ls -al ~/.ssh
```

### Lists the files in your .ssh directory, if they exist

检查目录列表以查看是否已经有 SSH 公钥。 默认情况下，公钥的文件名是以下之一：

id_rsa.pub
id_ecdsa.pub
id_ed25519.pub
如果您没有现有的公钥和私钥对，或者不想使用任何可用于连接到 GitHub 的密钥对，则生成新的 SSH 密钥。

如果您看到列出的现有公钥和私钥对（例如 id_rsa.pub 和 id_rsa），并且您希望使用它们连接到 GitHub，则可以将 SSH 密钥添加到 ssh-agent。

提示：如果您收到错误“~/.ssh 不存在”，不要担心！ 我们在生成新的 SSH 密钥时会创建它。

看是否存在 id_rsa 和 id_rsa.pub文件，如果存在，说明已经有SSH Key

如果提示没有SSH Key，不要担心！ 我们在生成新的 SSH 密钥时会创建它。

## 生成新 SSH 密钥并添加到 ssh-agent

检查现有 SSH 密钥后，您可以生成新 SSH 密钥以用于身份验证，然后将其添加到 ssh-agent。

如果您还没有 SSH 密钥，则必须生成新 SSH 密钥。 如果您不确定是否已有 SSH 密钥，请检查现有密钥。

如果不想在每次使用 SSH 密钥时重新输入密码，您可以将密钥添加到 SSH 代理，让它管理您的 SSH 密钥并记住您的密码。

### 生成新 SSH 密钥

打开 Git Bash。

粘贴下面的文本（替换为您的 GitHub 电子邮件地址）。

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

例如可以是

```bash
$ ssh-keygen -t ed25519 -C "cnyjzhang@outlook.com"
```

注：如果您使用的是不支持 Ed25519 算法的旧系统，请使用以下命令：

```bash
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

```

例如，

```bash
ssh-keygen -t rsa -C  "cnyjzhang@outlook.com"
```

这将创建以所提供的电子邮件地址为标签的新 SSH 密钥。

```bash
> Generating public/private ed25519 key pair.
```

提示您“Enter a file in which to save the key（输入要保存密钥的文件）”时，按 Enter 键。 这将接受默认文件位置。

```bash
> Enter a file in which to save the key (/c/Users/you/.ssh/id_ed25519):[Press enter]
```

在提示时输入安全密码。 更多信息请参阅[“使用 SSH 密钥密码”](https://docs.github.com/cn/free-pro-team@latest/articles/working-with-ssh-key-passphrases)。

```bash
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

当然，我这里并没有设置“使用 SSH 密钥密码”，直接回车就可以。

### 将 SSH 密钥添加到 ssh-agent

将新 SSH 密钥添加到 ssh-agent 以管理密钥之前，应检查现有 SSH 密钥并生成新 SSH 密钥。

如果已安装 GitHub Desktop，可使用它克隆仓库，而无需处理 SSH 密钥。

确保 ssh-agent 正在运行。 您可以根据“使用 SSH 密钥密码”中的“自动启动 ssh-agent”说明，或者手动启动它：

#### 在后台启动 ssh-agent

```bash
$ eval $(ssh-agent -s)
> Agent pid 59566
```

将 SSH 私钥添加到 ssh-agent。 If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_ed25519 in the command with the name of your private key file.

```bash
$ ssh-add ~/.ssh/id_ed25519
```

## 新增 SSH 密钥到 GitHub 帐户

要配置 GitHub 帐户使用新的（或现有）SSH 密钥，您还需要将其添加到 GitHub 帐户。

在新增 SSH 密钥到 GitHub 帐户之前，您应该已：

检查现有 SSH 密钥

生成新 SSH 密钥并添加到 ssh-agent

在新增 SSH 密钥到 GitHub 帐户后，您可以重新配置任何本地仓库使用 SSH。 更多信息请参阅[“将远程 URL 从 HTTPS 转换为 SSH”](https://docs.github.com/cn/free-pro-team@latest/articles/changing-a-remote-s-url/#switching-remote-urls-from-https-to-ssh)。

将 SSH 公钥复制到剪贴板。

如果您的 SSH 公钥文件与示例代码不同，请修改文件名以匹配您当前的设置。 在复制密钥时，请勿添加任何新行或空格。

```bash
$ clip < ~/.ssh/id_ed25519.pub
# Copies the contents of the id_ed25519.pub file to your clipboard
```

如果 clip 不可用，可找到隐藏的 .ssh 文件夹，在常用的文本编辑器中打开该文件，并将其复制到剪贴板。

```bash
vim ~/.ssh/id_ed25519.pub
# 或者
cat ~/.ssh/id_ed25519.pub

```

在任何页面的右上角，单击您的个人资料照片，然后单击 Settings（设置）。

用户栏中的 Settings 图标

在用户设置侧边栏中，单击 SSH and GPG keys（SSH 和 GPG 密钥）。

身份验证密钥

单击 New SSH key（新 SSH 密钥）或 Add SSH key（添加 SSH 密钥）。

SSH 密钥按钮

在 "Title"（标题）字段中，为新密钥添加描述性标签。 例如，如果您使用的是个人 Mac，此密钥名称可能是 "Personal MacBook Air"。

将密钥粘贴到 "Key"（密钥）字段。

密钥字段

单击 Add SSH key（添加 SSH 密钥）。

添加密钥按钮

如有提示，请确认您的 GitHub 密码。

Sudo 模式对话框

如下图
![](7cd5b32a.png)

GitHub点击用户头像，选择setting

新建一个SSH Key

取个名字，把之前拷贝的秘钥复制进去，添加就好啦。

## 测试 SSH 连接

设置 SSH 密钥并将其添加到您的 GitHub 帐户后，您可以测试连接。
测试 SSH 连接之前，您应已完成以下各项：

检查现有 SSH 密钥
生成新 SSH 密钥
新增 SSH 密钥到 GitHub 帐户
测试连接时，您将需要使用密码（即您之前创建的 SSH 密钥密码）验证此操作。 有关使用 SSH 密钥密码的更多信息，请参阅“使用 SSH 密钥密码”。

打开 Git Bash。

输入以下内容：

```bash
$ ssh -T git@github.com
# Attempts to ssh to GitHub
```

您可能会看到类似如下的警告：

```bash
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
> Are you sure you want to continue connecting (yes/no)?
```

验证您看到的消息中的指纹匹配步骤 2 中的消息之一，然后输入 yes：

```bash
> Hi username! You've successfully authenticated, but GitHub does not
> provide shell access.
```

验证生成的消息包含您的用户名。 如果收到“权限被拒绝”消息，请参阅[“错误：权限被拒绝（公钥）”](https://docs.github.com/cn/free-pro-team@latest/articles/error-permission-denied-publickey)。

## 使用 SSH 密钥密码

您可以保护 SSH 密钥并配置身份验证代理，这样您就不必在每次使用 SSH 密钥时重新输入密码。

使用 SSH 密钥时，如果有人获得您计算机的访问权限，他们也可以使用该密钥访问每个系统。 要添加额外的安全层，可以向 SSH 密钥添加密码。 您可以使用 ssh-agent 安全地保存密码，从而不必重新输入。

添加或更改密码
通过输入以下命令，您可以更改现有私钥的密码而无需重新生成密钥对：

```bash
$ ssh-keygen -p
# Start the SSH key creation process
> Enter file in which the key is (/Users/you/.ssh/id_rsa): [Hit enter]
> Key has comment '/Users/you/.ssh/id_rsa'
> Enter new passphrase (empty for no passphrase): [Type new passphrase]
> Enter same passphrase again: [One more time for luck]
> Your identification has been saved with the new passphrase.
```

如果您的密钥已有密码，系统将提示您输入该密码，然后才能更改为新密码。

在 Git for Windows 上自动启动 ssh-agent
您可以在打开 bash 或 Git shell 时自动运行 ssh-agent。 复制以下行并将其粘贴到 Git shell 中的 ~/.profile 或 ~/.bashrc 文件中：

```profile
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

如果您的私钥没有存储在默认位置之一（如 ~/.ssh/id_rsa），您需要告知 SSH 身份验证代理其所在位置。 要将密钥添加到 ssh-agent，请输入 ssh-add ~/path/to/my_key。 更多信息请参阅“生成新的 SSH 密钥并添加到 ssh-agent”

提示：如果想要 ssh-agent 在一段时间后忘记您的密钥，可通过运行 `ssh-add -t <seconds>` 进行配置。

现在，当您初次运行 Git Bash 时，系统将提示您输入密码：

```bash
> Initializing new SSH agent...
> succeeded
> Enter passphrase for /c/Users/you/.ssh/id_rsa:
> Identity added: /c/Users/you/.ssh/id_rsa (/c/Users/you/.ssh/id_rsa)
> Welcome to Git (version 1.6.0.2-preview20080923)
>
> Run 'git help git' to display the help index.
> Run 'git help ' to display help for specific commands.
```

ssh-agent 进程将继续运行，直到您注销、关闭计算机或终止该进程。

## 更改已有项目中的远程仓库地址

我们需要把之前的https连接改为ssh连接

修改项目目录下 .git文件夹下的config文件，将url地址替换

```bash
vim /.git/config
```

搜索替换,将

```conf
https://github.com/
```

替换成

```conf
git@github.com:
```

git  ssh地址获取可以看如下图切换。
![img](1690b889.png)
