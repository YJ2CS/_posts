---
title: Clion+Opencv3.2终极配置教程
url: clion-opencv-3.2终极配置教程
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - IDE
tags:
  - IDE
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-b744
date: 2020-11-10 22:16:00
---

# Clion+Opencv3.2终极配置教程

已剪辑自: https://blog.csdn.net/bskfnvjtlyzmv867/article/details/78940472

## 前言

网上的教程实在太坑，啰哩啰嗦还不对，很多感觉都是互相抄袭，也没有真正解决问题，抑或解决问题分享时草草了事，真是坑人！不多说了，还是正题吧…

## 环境

Opencv3.2+Clion+Win10

Cmake3.6(至少3.9版本一下)+Mingw-w64(64位的，32位的bug会出很多错)

需下载资源

	• 手动下载一个opencv_ ffmpeg_64.dll文件，放到opencv/sources/3rdparty/ffmpeg/目录下，下载地址：opencv3.2 opencv_ffmpeg_64。
	• 如果是需要opencv_ ffmpeg.dll，也需要放到opencv/sources/3rdparty/ffmpeg/目录下，下载地址：opencv_ ffmpeg.dll。
	
建议都直接下载好放进去，省着出错麻烦！！！

## 编译Opencv源码步骤

1. 安装Opencv3.2，Cmake以及Mingw-w64， 配置Mingw-w64的bin目录环境变量；

2. 打开Cmake-GUI，源码路径选择Opencv的source目录，输入路径自定义，如图；

![](images/f1a5fd83.png)
![](images/ba5b3870.png)

3. 点击Configure，选择MinGW Makefiles；

![](images/bb256dc3.png)

4. 再次点击Configure，等待一会会很多报红，如图；再次点击Configure，红色全部消失；此时点击Generate完成即可；

![](images/ae2ece96.png)
![](images/5aaaa19d.png)

5. 进入输出目录，如果安装了git的话，可以直接git-bash里（或者cmd）里运行下面代码，效果如下：

mingw32-make -j8 # 以8线程进行编译

![](images/11a90c72.png)

6. 等待一会，即可完成，最终效果如下：

![](images/ee8a7dbb.png)

7. 最后在我们编译完成，输出目录下的bin目录里会生成一些.dll和.exe文件，lib目录会生成一些.a文件。

![](images/49aade77.png)
![](images/2620c26c.png)

8. 运行mingw32-make install，等待片刻，输出目录下会多出install文件夹； 

![](images/dc41dd7a.png)

9. 添加…\install\x86\mingw\bin 添加到path系统环境变量环境变量；

![](images/898bdcd4.png)

Clion中使用Opencv
1. 安装Clion，配置好Mingw-w64的目录(包括Cmake，可选)；

![](images/fd90c6c5.png)

2. 新建项目，发现Cmake3.9一创建项目就报错，所以上一步还是不要选择Bundle的，我自己又下载了一个3.6版本的，心累…
 
![](images/15f05e9d.png)

3. 编辑CMakeLists.txt；

cmake_minimum_required(VERSION 3.6)

project(opencvtest)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# Where to find CMake modules and OpenCV
```
set(OpenCV_DIR "E:\\Opencv3.2\\opencv\\mingw64_build\\install")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(OpenCV REQUIRED)

include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(opencvtest main.cpp)

# add libs you need

set(OpenCV_LIBS opencv_core opencv_imgproc opencv_highgui opencv_imgcodecs)

# linking

target_link_libraries(opencvtest ${OpenCV_LIBS})

```

4. 测试代码main.cpp
```
#include "iostream"

#include<opencv2/opencv.hpp>

using namespace std;

using namespace cv;

int main() {

Mat img = imread("haha.jpg");

if (img.empty()) {

cout << "Error" << endl;

return -1;

}

imshow("Lena", img);

waitKey();

return 0;

}
```

5. 结果：

![](images/63ba2599.png)

6. 中间坑真的感觉数不清，配置出现差错可以休息一下，重启一下电脑，说不定就好了：）

参考文章

很多都是大坑，这里就列两个主要的吧！

Win10下Clion配置opencv3

https://blog.csdn.net/xiangxianghehe/article/details/70880762

如何在CLion上配置使用OpenCV？

https://www.zhihu.com/question/47331502

基于ClIon的CMake、MinGW与Cygwin配置简易指南

已剪辑自: https://blog.csdn.net/u013023297/article/details/80723847

Clion是捷克公司JetBrains出品的JB全家桶之中主要面向C、C++的集成开发环境。JB家的Pycharm和IDEA是其最为出名的两款，而我也是从Pycharm入的坑。因为近期想再把C捡起来复习，在网上看了好些相关IDE推荐，最后还是选择Clion。

Clion与CodeBlocks等不太一样的地方在于，官方允许基于MinGW、Cygwin与VisualStudio三种工具链进行设置，在此仅对前两种进行介绍。
```

硬件配置：Dell游匣笔记本7559，CPU:i5-6300HQ，内存：8G。
系统配置：Win10专业版1803
软件配置：JetBrains Clion 2018.1.3 
MinGW 2013-10-26 
Cygwin 2.10.0 
CMake3.11.4
```

首先安装CMake、Cygwin与MinGW。

CMake安装

其中CMake是最容易也最傻瓜的。搜索官网，下载对应平台的版本，这里建议直接下载.msi的安装版而非.zip的绿色版，从而免除手动配置环境变量。简单地说就是勾选同意协议、将CMake添加到面向所有使用者的系统路径(Add CMake to the system PATH for all users)。安装路径可根据自己需要修改。随后next即可。

正确安装后，在命令行输入cmake会弹出如下界面: 

![](images/24d1dd26.png)

MinGW安装

从官网先下载好安装的下载器，需要设置如下所示的版本、基于的架构等。 

![](images/565873a1.png)
![](images/d2d18859.png)

next后则可设置安装目录，个人喜欢在D盘下新建一个MinGW文件夹，这样方便管理与查找。

![](images/ea855acb.png)

接下来一路next即可。

在桌面上会出现一个下图所示图标，双击即可进入MinGW的包管理。

![](images/d29f7277.png)

安装界面如下所示，

![](images/825facd8.png)

此处我们以Basic Setup中的mingw32-base为例介绍安装流程。

点击左侧的Basic Setup，鼠标移动至mingw32-base处，右键选中Mark for Installation，如下图所示。

![](images/449a6530.png)

属性栏有Package（包名）、Class（类别，一般需要选择bin与dev的）、InstalledVersion（安装版本，仅勾选后才会显示）、Repository Version（与前者类似）、Describtion（说明，对包的用途进行简单介绍）

再点击左侧菜单栏中的Installation，下拉选中Apply Changes，会弹出如下所示的窗

![](images/fc827c6c.png)

点击Apply，随后软件自动进行下载安装，完成后也会弹出类似日志的细节提示，说明哪些压缩包已被安装。

![](images/07812e5c.png)

![](images/f15dd80e.png)

![](images/5c72b03f.png)

若要进行C/C++开发，原则上需要安装的MinGW包如下所示
```

Basic Setup部分
mingw32-base
mingw32-gcc-g++
All Packages部分
```

主要集中在MinGW Base System分支下

```
勾选需注意class
mingw32-gcc         c编译器
mingw32-gcc-g++     c++编译器
mingw32-gdb         debugger
```

验证是否安装成功，则可以在命令行中输入gcc

出现下述界面即说明安装成功。 

![](images/9c5a73eb.png)

Cygwin安装

相对而言Cygwin安装是最麻烦的，因为本身Cygwin就是许多自由软件的集合，在Windows上运行类UNIX系统。体积和规模就比MinGW与CMake大了不少。

Cygwin安装方法类似MinGW，从官网下载下来后直接打开，一路next，中间会需要选择下载方式，一般选择Install from Internet（即第一种），然后选择ROOT的安装目录（此处照旧新建Cygwin然后把东西扔里面），接下来需要选择安装包的本地下载位置，此处新建一个IDM文件夹然后选择它，对于选择连接方式，默认直连，短暂跳过一个页面后，会出现下载地址的选择，一般来说国内公网选择163的站点没问题，网易的Cygwin镜像使用帮助对使用说明进行了详细的介绍，阅读并跟随操作即可，教育网则可选择清华TUNA源、中科大源等高校镜像站点，使用VPN的用户建议根据目标国家进行选择，或暂时退出VPN。

然后就会自动进入下一步，弹出选择窗口。在搜索小窗处输入gcc-core、gcc-g++、make、gdb、binutils，并在devel下点击skip，在Bin属性下出现勾选的方框即可。

若有其他需求，则根据需要搜索相对应的包并安装，例如笔者就额外增加并安装了Python模块、net下的openssl、openssh等，此处增加的包/模块越多，安装的时间也就越长。

接下来就是漫长的等待，其需要经历下载——安装——启动三个阶段。

完成后会自动勾选Create icon on Desktop，点击完成即可。

Clion安装与配置

接下来就是最重要的ClIon安装配置。

安装很简单，例行配置目录以及启动项。关键是配置，JB家软件的配置逻辑基本都是先进行主题配置再进行运行相关的配置。

UI主题根据爱好选择Decula或者InteliJ，

然后就进入工具链设置。

![](images/95d22da6.png)

首先设置MinGW的，左侧点击加号，环境选择MinGW，点击右侧的按钮，将MinGW文件夹设为目标路径。

然后照例设置CMake、Make、C Compiler和C++ Compiler。其中由于MinGW的Make可执行程序名字为mingw32-make.exe，故复制一个副本然后更名为Make.exe，Make的路径直接选中它。

设置好MinGW的文件路径后，软件会自动探测对应Make、C Compiler和C++ Compiler的可执行程序，但速度略慢，可等待也可手动选择。

设置完成后，可对语言插件进行设置，这里只设置Markdown的支持，其他保持默认状态。

下面进行HelloWorld测试

新建一个工程，直接Run

![](images/adc47855.png)

顺利输出Hello，world!

![](images/8028fe12.png)

设置Cygwin的配置

大体类同于MinGW，在菜单栏中选中File，单击Settings，则会弹出一个窗口，选择Build,Execution，Deployment分支，点击下属的Toolchains则会弹出刚才设置MInGW的窗口。

![](images/d8746c7e.png)

点击+号，依次对Enviroment类型、路径，以及CMake、Make、C Compiler、C++ Compiler以及Debugger进行设置，其中一旦设置了Enviroment的路径，CMake与Debugger则会自动选中（即CMake可不使用额外安装的版本，而且Cygwin下使用额外安装的CMake会弹出警告提示），设置完毕后可点击左侧的向上箭头将Cygwin设为默认选项

下面进行输出测试

新建一个工程，直接Run

![](images/77b1ebae.png)

![](images/1443487a.png)

顺利输出Hello，Louis!

至此基本配置完成。
