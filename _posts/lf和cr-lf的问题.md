---
title: LF、CRLF浅析
url: lf和cr-lf的问题
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-63d0'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-23T00:11:58+08:00'

---

## LF、CRLF

“回车”（`CR` ，Carriage Return）和“换行”（`LF` ，Line Feed）

### 不同平台

-   Dos 和 windows 采用***“回车+换行，CR/LF”***表示下一行；
-   UNIX/Linux 采用***“换行符，LF”***表示下一行；
-   苹果机(MAC OS 系统)则采用***“回车符，CR”***表示下一行。

Linux使用的是LF
早期mac os使用的是CR,后来版本更新，与Linux同步，都使用了LF
Windows使用的是CRLF

### 历史原因

在早期，键盘的原型”打字机“（电传打字机，Teletype Model 33，Linux/Unix下的tty概念也来自于此）有着两个不同的键，CR和LF

”换行”（LF），告诉打字机把纸向下移一行。

“回车”（CR），告诉打字机把打印头定位在左边界

LF代表换到下一行，而CR代表回到行首。

在那个时代，换行到了下一行，并不代表就是自动到下一行的行首，所以实际上人们在换行的时候要按分别的两个键CR、LF。

继承到了计算机时代，Windows因为这个历史原因，选择了使用CRLF表示换行的这个操作。

而Linux使用了LF，Mac OS在版本更新后也使用了LF。

这就为我们程序开发人员跨平台合作开发带来了阻力，我在Windows上使用的CRLF，到了你Linux上怎么办？不就成了换行加上一个多余字符"LF"吗？为了解决这个问题，版本管理工具Git有自带的自动替换功能，

具体来说，默认情况下，在Windows段开发的开发人员，在进行 _git commit_ 时你的换行符会被统一替换成"CRLF"。而到了Linux上，在进行 _git commit_ 时你的换行符会被统一替换成"LF"，所以大多数情况下，开发人员不再需要关心这个问题，就可以正常开发。

但有一个问题，这个统一替换的过程，会自动的体现在命令行上，稍微大一点的项目，就会输出成千上万行的”自动替换提升,LF to CRLF“,这就对开发人员阅读命令行的内容造成了困扰。

此外，很明显，这个转换过程需要挤占你系统的大量资源，体现出来，就是内存占用会飙升，持续百分百占用。

好在，Git的这些默认设置是可以手动修改的。

理想的修改是，

Windows平台开发人员，将IDE默认的换行符改为LF(如我使用的IDEA)。

Git修改为不再进行自动转换，

同时设置，Git在进行 _git commit_ 时也不会去检查是否存在LF和CRLF混用的问题。

这样的设置只需要几行代码。但也要注意，设置过后，开发者就需要自己检查，文件的换行符有没有混用。换行符混用，是很容易造成一些开发工具不能正常启动的。

### 设置新建项目、新建文件的默认换行符格式

在File -> settings -> Editor -> Code Style中修改，如图：

![image-20201116174347226](image-20201116174347226.png)

<img src="image-20201116174309793.png" alt="image-20201116174309793" style="zoom: 67%;" />

![image-20201116174309793.png](images/image-20201116174309793.png)

### 对已有文件，CRLF与LF之间进行批量转换

**如果没有强制需求，不要把已经能跑起来的项目，批量转换换行符格式**

如果你的项目现在能够正常跑起来，我建议你千万不要去进行批量转换，毕竟没改的话，你只是会看到几千行Git的检查提示，而更改以后，你则会更高几率，直接代码跑不起来。相信我，那时候你一定会很痛苦。

在IDEA中，首先选中一个根文件夹，之后便会遍历其所有子文件夹和子文件，将其中换行符进行批量替换。

详细操作：

例如，首先我们选择la-blog这个根文件夹，其所有子文件夹和子文件都会在随后操作中批量替换换行符。

![image-20201116173730760](image-20201116173730760.png)

选择File -> File options -> line Separator  ，在其中就可以选择你需要的换行符格式了。

![image-20201116173934092](image-20201116173934092.png)

## Git设置

```git
# 提交时转换为LF，检出时转换为CRLF
git config --global core.autocrlf true

# 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

其他选项

### AutoCRLF

```git
# 提交时转换为LF，检出时转换为CRLF
git config --global core.autocrlf true

# 提交时转换为LF，检出时不转换
git config --global core.autocrlf input

# 提交检出均不转换
git config --global core.autocrlf false
```

### SafeCRLF

```git
#拒绝提交包含混合换行符的文件
git config --global core.safecrlf true

#允许提交包含混合换行符的文件
git config --global core.safecrlf false

#提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```
