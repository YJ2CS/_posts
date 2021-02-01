---
title: win10环境下-mingw-w64-cmake-clion-安装及配置-cpp-和-opencv-运行环境
siteurl: win10环境下-mingw-w64-cmake-clion-安装及配置-cpp-和-opencv-运行环境
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 开发环境
tags:
  - 环境配置
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-6264'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-23T00:21:47+08:00'

---
# Win10 环境下 Mingw-w64，CMake，CLion 安装及配置 C/C++ 和 OpenCV 运行环境

最近在 `windows` 环境下进行 `C/C++` 的开发，花了很长时间进行环境的配置

记录下如何安装 `CLion`，`Cmake`，`MinGW-w64`，配置 `C/C++` 开发环境以及 `OpenCV` 开发环境

当前运行环境：`Win10`

*好像使用 `CLion` 还需要独立安装 `cmake`，不过我之前已经安装过了，所以就不记录了*

------

主要内容：

- 工具下载
- `MinGW-w64` 安装
- `CMake` 安装
- `CLion` 安装和 `C/C++` 环境配置
- `OpenCV` 配置

------

# 工具下载

- [CLion 下载](http://www.jetbrains.com/clion/)
- [MinGW-w64 下载](https://sourceforge.net/projects/mingw-w64/)
- [CMake 下载](https://cmake.org/download/)
- [OpenCV 下载](https://opencv.org/releases.html)

------

# `MinGW-w64` 安装

首先安装 `MinGW-w64`，下载完成后，一路默认安装即可

安装完成后还需要将 `bin` 路径加入到环境变量 `PATH` 中

```
C:\Program Files\mingw-w64\x86_64-7.3.0-posix-seh-rt_v5-rev0\mingw64\bin
1
```

配置好后，打开命令行窗口输入 `gcc -v` 和 `g++ -v` 进行测试：



![这里写图片描述](images/2018050616502437.jpg)




![这里写图片描述](images/20180506165034510.jpg)


------

# `CMake` 安装

首先从官网下载 `cmake`，选择 `Binary distributions:` 下面 `zip` 版本：



![这里写图片描述](images/20180506165101717.jpg)


下载完成后，直接解压，将其 `bin` 目录加入在环境变量 `PATH` 中即可：

```
C:\software\cmake\cmake-3.11.1-win64-x64\bin
1
```

在命令行窗口中输入命令 `cmake --version` 进行测试



![这里写图片描述](images/20180506165112301.jpg)


## `anaconda cmake`

在安装过程中发现，在 `anaconda` 中也有 `cmake`，在命令行窗口中显示的是 `anaconda-cmake` 的版本，将其路径在环境变量 `PATH` 中去除即可

------

# `CLion` 安装和 `C/C++` 环境配置

下载好 `CLion` 后，同样默认安装即可，如何之前已经安装好 `MinGW-w64`，那么软件会默认检测到该配置

这里还有一个问题，就是之前在环境变量中配置了 `git`，那么会提示出错。我当时忽略了这个提示，一路安装结束，新建工程后发现还是提示这个错误：



![这里写图片描述](images/20180506165128355.jpg)


在环境变量 `PATH` 中去除 `git` 的 `bin` 路径后，再次刷新，工程才能够正确运行

*想要使用 `git`，可以打开 `git bash` 使用*

## 参考

[idea开发工具(PyCharm IntelliJ Clion等)全家桶注册码(有效到2019)](https://blog.csdn.net/qq_29232943/article/details/79516472)

------

# `OpenCV` 配置

使用 `Mingw-w64` 进行 `OpenCV` 库的编译是最麻烦的事情，常常出现意想不到的错误，在网上也找不到相应的解答

当前按照网上的教程，使用 `Opencv3.2` 完成编译配置

使用工具：

```
cmake
mingw-w64
12
```

## `OpenCV` 库编译

下载 `OpenCV`，解压，进入该文件夹后新建文件夹 `mingw64-build`

打开 `cmake-gui`，配置如下：



![这里写图片描述](images/20180506165140889.jpg)

*在 `where is the source code` 编辑框中选择 `source` 文件夹*
*在 `where is the binaries` 编辑框中选择 `mingw64-build` 文件夹*

- 点击下方的 `Configure`，等待配置结束
- 再次点击 `Configure`，等待配置结束
- 点击按钮 `Generate`，等待生成结束

这几步完成后，在 `mingw64-build` 文件夹内应该有很多文件了

打开命令行窗口，进入 `mingw64-build` 路径，输入如下命令：

```
mingw32-make -j8
1
```

**获取打开 `git bash`，进入 `mingw64-build` 路径后输入 `mingw32-make.exe -j8`**

等待完成，再输入命令：

```
 mingw32-make install
1
```

等待完成后 `OpenCV` 库的编译即算完成

### 环境配置

将 `bin` 路径加入到系统环境变量 `PATH` 中

```
C:\software\opencv\opencv3.2\opencv\mingw-build\bin
1
```

### 错误一：在 `make` 阶段遇到如下错误

```
mingw32-make[2]: *** [modules\python2\CMakeFiles\opencv_python2.dir\build.make:180: modules/python2/CMakeFiles/opencv_python2.dir/__/src2/cv2.cpp.obj] Error 1
mingw32-make[1]: *** [CMakeFiles\Makefile2:6794: modules/python2/CMakeFiles/opencv_python2.dir/all] Error 2
mingw32-make[1]: *** Waiting for unfinished jobs....
[100%] Linking CXX executable ..\..\bin\opencv_perf_stitching.exe
[100%] Built target opencv_perf_stitching
mingw32-make: *** [Makefile:162: all] Error 2
123456
```

解决方法，重新打开 `cmake-gui`，取消选项 `BUILD_opencv_python2`



![这里写图片描述](images/20180506165156387.jpg)


## CLion 配置

打开 `CLion`，新建工程，配置 `CMakeList.txt` 文件如下：

```
cmake_minimum_required(VERSION 3.10)
project(FIrst)

set(CMAKE_CXX_STANDARD 11)

add_executable(FIrst main.cpp)

# Where to find OpenCV

set(OpenCV_DIR "C:\\software\\opencv\\opencv3.2\\opencv\\mingw-build\\install")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(OpenCV REQUIRED)

include_directories(${OpenCV_INCLUDE_DIRS})

# add libs you need

set(OpenCV_LIBS opencv_core opencv_imgproc opencv_highgui opencv_imgcodecs)

# linking

target_link_libraries(FIrst ${OpenCV_LIBS})
123456789101112131415161718192021222324
```

*Note：修改相应的工程名和 `OpenCV` 路径*

## 参考

[Clion+Opencv3.2终极配置教程](https://blog.csdn.net/bskfnvjtlyzmv867/article/details/78940472)

[如何在CLion上配置使用OpenCV？](https://www.zhihu.com/question/47331502)