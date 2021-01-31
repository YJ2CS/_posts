---
title: Windows下的基础环境配置
url: windows下的基础环境配置
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-63d1'
date: 2020-11-22T20:16:00.000Z
date updated: '2020-12-26T19:53:08+08:00'

---

## 基本设置

win+i打开设置，个性化，颜色，配置暗色主题。

相比mac，win上界面的默认字号有点大，可以在显示设置->缩放与布局中降低缩放比例，比如我在4k显示时一般使用150%，在用内置显示器时是175%，设置完后会自动记忆，不需要反复设置。

后文中会提到一些编程使用的等宽字体相关配置，我一般会选择像FiraCode，PT Mono等字体，大家可以凭个人喜好选择：[Programming Fonts](https://link.zhihu.com/?target=https%3A//www.programmingfonts.org/)

Windows上的非衬线字体，一般默认用微软雅黑，衬线字体，我选择思源宋体：[Source Han Serif](https://link.zhihu.com/?target=https%3A//source.typekit.com/source-han-serif/)

字体配置方面，IDE，Terminal下面会提到。浏览器的话可以在Edge/Chrome的设置里找到：

![img](https://pic1.zhimg.com/v2-586b55e3533edea371f1084dd1a3e5fb_r.jpg?source=1940ef5c)

电源设置中，可以配置合上笔记本盖子而不休眠，外接显示器时有用。

触控板使用中，禁掉双击拖动可多选，会减少一些误操作。

Airpods要使用麦克风时，需要切到hands-free模式: [airpods的stereo和air hands-free两种模式有什么区别？](https://www.zhihu.com/question/281707821/answer/927141151) 。

在使用过程中，发现本本偶尔会出现长时间睡眠后唤醒出现黑屏问题。排查了一番感觉跟快速启动，休眠模式这些会把内存dump到硬盘上的功能有关。目前的解决方案是在电源管理中关闭快速启动（其实用了ssd速度没啥差别），
另外在powershell中执行`powercfg /H off` 来关闭休眠功能（还能省出几个G的磁盘空间）。

## 2. Shell相关配置

### 2.1 WSL

安装WSL，可以参考: [在 Windows 10 上安装适用于 Linux 的 Windows 子系统 (WSL)](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/wsl/install-win10%23update-to-wsl-2) 。
WSL有2个版本，我目前用的是2，总体性能比1好，但是跨操作系统的文件读写性能比1差。具体如何选择，以及WSL下的最佳实践，可以参考: [比较 WSL 2 和 WSL 1](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/wsl/compare-versions) 。

安装完WSL，可以在Windows Store中安装Linux发行版，我目前装的是Ubuntu，有点想尝试Manjaro哈哈。

WSL Ubuntu中安装开发环境，zsh, oh-my-zsh, starship, tmux等，可以参考: [https://towardsdatascience.com/the-ultimate-guide-to-your-terminal-makeover-e11f9b87ac99](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/the-ultimate-guide-to-your-terminal-makeover-e11f9b87ac99) 。

### 2.2 PowerShell

包管理：跟mac一样，Win 10下安装软件，我会先在Store里找，如果没有再考虑命令行中的包管理软件。这样做的好处是各种版本更新会比较方便。包管理软件我选择Scoop: [lukesampson/scoop](https://link.zhihu.com/?target=https%3A//github.com/lukesampson/scoop) ,
后续可以考虑官方的winget: [Windows 程序包管理器](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/package-manager/) 。
完成安装后，在Scoop中安装常用命令行工具，如sudo, git, 7zip, vim等。如果希望能在windows的powershell里拥有类似Linux的体验，也可以考虑安装gow(GNU on Windows)。
个人目前的打算是学习一下powershell，或者直接在WSL中做复杂脚本的开发，所以也没有考虑cygwin, msys2, mingw等。

连开发服务器最常用的就是ssh了，现在已经Win 10自带了ssh client(server默认没有装，需要在windows可选功能中启用)，一般在C:\Windows\System32\OpenSSH这个位置，加入PATH环境变量，然后就可以使用各类ssh工具，包括个人ssh profile(key, config等)的配置了。

PowerShell指南: [PowerShell 文档 - PowerShell](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/powershell/) ，目前默认是5.x版本，可以手动安装最新的7.x。

### 2.3 Terminal应用

Terminal应用方面，默认的cmd跟powershell外观简直不能看，putty，Xshell这些感觉也是上个世纪的UI。可以在Windows Store中搜索安装Windows Terminal，另外[Hyper](https://link.zhihu.com/?target=https%3A//hyper.is/)也是个不错的选择。安装完之后照例做一系列美化工作: [Windows 终端 Powerline 设置](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/terminal/tutorials/powerline-setup) ，记得装Cascadia Code PL或者其它的powerline字体 :)

Mac到Windows的快捷键操作指南: [从 Mac (Unix) 转到 Windows 的帮助](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/dev-environment/mac-to-windows) 。Termnial中移动光标的方法，移到开头Home，移到结尾End，按单词移动ctrl + 左右方向键，按单词删除ctrl + backspace/delete。如果是内置笔记本键盘，可能需要使用Fn键来触发Home, End等键的效果。

![img](https://pic1.zhimg.com/v2-3e6561c91d4622ef9ff832ea565b0c65_r.jpg?source=1940ef5c)Windows Terminal分屏效果

## 3. 常用软件

网络: [Fndroid/clash_for_windows_pkg](https://link.zhihu.com/?target=https%3A//github.com/fndroid/clash_for_windows_pkg) ,
注意设置一下UWP Loopback。在下载其它软件包或代码时，一般可以用两类方法加速：

- 挂代理，例如git config https.proxy命令
- 配置国内源，例如著名的pip清华源，方法与在mac/linux下基本一样

类似mac spotlight的替代，安装Microsoft PowerToys([microsoft/PowerToys](https://link.zhihu.com/?target=https%3A//github.com/microsoft/PowerToys))，如果希望拥有Alfred那样强大的能力，也可以考虑选择listray或utools。

类似mac的快速预览，可以在Windows Store安装QuickLook。

PDF阅读，Windows Store安装Xodo。epub阅读，安装Neat Reader，国际版貌似广告少点。好多人推荐[sumatra](https://link.zhihu.com/?target=https%3A//www.sumatrapdfreader.org/free-pdf-reader.html)，一个软件支持pdf, epub, mobi，而且很轻量快速！不过颜值有点低，被我抛弃了……

笔记软件，个人选择Notion。当然OneNote，印象笔记也可以。甚至可以尝试下最近有点火的：[玩转 Obsidian |  打造知识循环利器 - 少数派](https://link.zhihu.com/?target=https%3A//sspai.com/post/62414) 。

Dash的替代品，可以考虑Zeal，或者DevDocs：[egoist/devdocs-desktop](https://link.zhihu.com/?target=https%3A//github.com/egoist/devdocs-desktop) 。

媒体播放，scoop install mpv，非常干净简洁。

评论区有同学提出强烈的干净好用的音乐播放器的需求，我这里推荐：[Listen 1 音乐播放器](https://link.zhihu.com/?target=https%3A//listen1.github.io/listen1/) 不知道是否有更好的替代。

云盘同步，OneDrive，容量大价格也便宜。Windows上的Office全家桶还是比Mac流畅不少啊。如果常做ppt，可以考虑装个iSlide。

如果不确定是否靠谱的软件，可以在沙盒中安装尝试。

## 4. 特定开发环境

安装vscode, PyCharm等IDE环境。vscode下结合wsl的开发指南: [Developing in the Windows Subsystem for Linux with Visual Studio Code](https://link.zhihu.com/?target=https%3A//code.visualstudio.com/docs/remote/wsl) 。IntelliJ也在积极适配WSL2，比如目前的版本可以直接打开WSL中的项目，且IDE会自动适配使用WSL内部的git命令等: [IntelliJ IDEA 2020.2 EAP3: Support for Git Installed in WSL2, Java Completion Improvements, and More – IntelliJ IDEA Blog | JetBrains](https://link.zhihu.com/?target=https%3A//blog.jetbrains.com/idea/2020/06/intellij-idea-2020-2-eap3-support-for-git-installed-in-wsl2-java-completion-improvements-and-more/) 。

另外也有干脆用WSL2跑GUI app的方式来运行IntelliJ的做法，参考这两篇：[https://itnext.io/using-wsl-2-to-develop-java-application-on-windows-8aac1123c59b](https://link.zhihu.com/?target=https%3A//itnext.io/using-wsl-2-to-develop-java-application-on-windows-8aac1123c59b) ，[https://medium.com/@ragin/development-under-windows-under-linux-with-wsl2-intellij-860daf601b61](https://link.zhihu.com/?target=https%3A//medium.com/%40ragin/development-under-windows-under-linux-with-wsl2-intellij-860daf601b61) 。

Python安装指南: [开始在 Windows 上使用 Python（初学者）](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/python/beginners) 。Miniconda安装，从官网下载即可。

JDK安装，我目前选择从graalvm官网下载安装包，并设置环境变量。

CUDA on Windows: [CUDA Toolkit Documentation](https://link.zhihu.com/?target=https%3A//docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html) 。

安装Docker: [Docker Desktop WSL 2 backend](https://link.zhihu.com/?target=https%3A//docs.docker.com/docker-for-windows/wsl/) 。

Web请求模拟，安装Insomnia: [Insomnia](https://link.zhihu.com/?target=https%3A//insomnia.rest/) 。

有一些教程中建议增加排除项或者关闭Windows Defender来提升项目build的性能：[通过更新 Defender 设置提高性能速度](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/android/defender-settings) 。

## 5. 常用快捷键

全局搜索，win + s，配合索引配置感觉基本够用，就没有额外安装everything。

睡眠，win + x, u, s。

系统自带截图，win + shift + s。

任务视图，win + tab，切换虚拟桌面win + ctrl + 左右方向键。

窗口布局管理，win + 方向键，可以很方便的实现最大化，或者占据一半屏幕。

剪切板管理，win + v，mac下类似功能貌似需要安装额外软件才能支持。

其它像win + r运行，win + e打开文件管理器，win + d显示桌面，win + r运行等也较为常用。

![img](https://pic4.zhimg.com/v2-b0e0925066c2e7973f14c51ea9f9a032_r.jpg?source=1940ef5c)长按win键会显示快捷键提示

## 6. 其它

一系列开发者的文章，有不少是从mac转到了windows，各种优缺点写的都还挺中肯，供大家参考: [从 Mac 切换到 Windows 的开发人员的成功案例](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows/dev-environment/dev-stories) 。其中有一篇是名人DHH写的，可以看出近一年来windows在开发者这块的投入及变化也是非常大。

作者：字节
链接：<https://www.zhihu.com/question/324218869/answer/1575858277>
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
