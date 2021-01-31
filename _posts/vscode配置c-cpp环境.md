---
title: 挑把趁手的兵器——VSCode配置C/C++学习环境（小白向）
url: vscode配置c-cpp环境
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-ec6e'
date: 2020-11-16T17:54:00.000Z
date updated: '2020-12-26T11:46:23+08:00'

---

转载: [冷却](https://www.zhihu.com/people/isunjn)

> 这是一篇面向小白的vscode教程，内容详细，帮你搭建自己的语言学习环境

很多大学的计算机专业用的入门语言都是C语言，通常老师会给学生指定一款IDE来进行程序的编写，比如vc++6.0、Code::Blocks、Dev c++，可是这些IDE大都比较老旧，用起来有很多不顺畅的地方，而且界面粗糙，一点都没有印象中程序员该有的那种炫酷的感觉，所以寻找一款现代化的、功能强大的编辑器/IDE对于一些人来说还是很有必要的。

也许有人说这些IDE开箱即用，不需要额外配置，对于什么都还不了解的新人来说很合适。我认为这是有道理的，但不应当妨碍一个有好奇心和折腾欲的学生去尝试其他的编程工具，我个人认为爱折腾对计算机专业的学生来说是一项可贵的品质，折腾工具、搭建环境的过程中可以学到很多有用的东西，这是与计算机交流的过程，也是每一个进入代码世界的人的必经之路，只是要学会克制，不要把时间全花在折腾工具和环境上。

拥有一套自己精心配置的编程工具，可以帮助计算机新人更快地走进代码的世界，提高对编程、对学业的兴趣。

目前网上有大量的关于vscode的文章和教程，但我没有找到一篇细致的、面向小白的、搭建语言学习环境而非实际开发环境的教程，很多教程只是写了怎样配置，却没有写为什么要这样配置，我自己搞清楚其中种种后决定记录下来。

### 为什么是VSCode

VSCode是微软出品的轻量级编辑器，定位是文本编辑器，开源，免费，海量插件，外观出色，简洁流畅，支持众多编程语言，支持三大操作系统Windows、Linux、MacOS。**总之，这是一款足够强大和优雅的编辑器，你值得拥有。**

> 与VScode(Visual Studio Code)名字相像的VS(Visual Studio)是微软的IDE，而VScode是编辑器，两者定位不同，一个蓝色一个紫色，不要搞混了。
>
> 我的环境是windows10, linux和mac os可能不适用，但如果你是小白的话，我认为其中很多内容还是很有参考意义的
>
> 本文以C语言为例，C++同理，涉及到不同的地方有标注

让我们开始这场vscode的配置之旅

## Step 0 基本概念

小白之所以是小白，就在于很多东西不知道、不了解、没见过、没用过，在你配置编程工具的过程中，你会遇到很多课本里没有、老师课上没说的东西、概念，这也是为什么很多人说配置vscode太麻烦了的原因，这种时候，善用搜索引擎，遇到不懂的东西、没见过的名词，上网查一下，大概了解一下是什么再接着往下看就行了。

在这篇文章中，我会尽量解释清楚每个对你来说可能陌生的东西，不过仍然有可能会有你不明白的地方，网上查一下就好。

最一开始，你应当了解如下概念：

-   **编程是怎样的一个过程：**

![img](https://pic3.zhimg.com/v2-47c1cbca6e8f38c90b9b1a3f77adce56_r.jpg)

首先用文本编辑器编写源代码 -> 编译源代码 生成目标代码-> 将目标代码与其它代码（如库函数代码、标准启动代码）链接起来 -> 生成可执行代码

-   **区分编辑器、编译器、IDE：**

**编辑器**就是处理文本（源码）的程序，写代码写的就是文本，编辑器可能提供智能提示、代码高亮等辅助功能，但不负责源码到二进制文件的操作；

**编译器**就是负责将源码文本翻译成计算机能够理解和执行的二进制文件的程序；

**集成开发环境**（IDE，Integrated Development Environment ）是用于提供程序开发环境的应用程序，包括了代码编辑器、编译器、调试器和图形用户界面工具。集成了代码编写、分析、编译、调试等一整套工具链。

-   **什么是搭建环境：**

vscode定位代码编辑器，不是IDE，不包含编译功能，因此需要我们自己安装编译器、调试器等编译器套件，并使两者有效的配合起来，以实现快捷操作。把这一整套工具链整合到一起的过程就是我们所说的搭建环境。

ok,到这里，我们就清楚要做什么了：获取编辑器 -> 获取编译套装(编译器、调试器、头文件库等) -> 做好两者之间的沟通工作(配置文件)

## Step 1 下载安装

两个东西：编辑器和编译套装

编辑器就是我们的vscode了，到官网

[Visual Studio Code - Code Editing. Redefinedcode.visualstudio.com![图标](https://pic4.zhimg.com/v2-beaba009c542a9f6fe1d2034a7ed568b_180x120.jpg)](https://link.zhihu.com/?target=https%3A//code.visualstudio.com/)

下载安装：

![img](https://pic3.zhimg.com/v2-68544ffce18f7902d7df0f5ad4556f42_r.jpg)

双击打开下载好的程序进行安装，安装到默认位置或者你自定义的位置，安装过程中注意这个界面：

![img](https://pic4.zhimg.com/v2-3d3816a67d2ff23d71212203d917e25b_r.jpg)

这几个选项建议全部勾上。

然后是编译套装

编译工具我们选用gcc（全称GNU Compiler Collection 意思是GNU编译器套件），不过不是原版的gcc，而是它在Windows下的特制版**MinGW**(全称Minimalist GNU on Windows）。它实际上是将GCC 移植到了 Windows 平台下，并且包含了 Win32API ，因此可以将源代码编译为可在 Windows 中运行的可执行程序。而且还可以使用一些 Windows 不具备的，Linux平台下的开发工具。MinGW又分为MinGW-w64 与 MinGW ，区别在于 MinGW 只能编译生成32位可执行程序，而 **MinGW-w64** 则可以编译生成 64位 或 32位 可执行程序。MinGW 现已被 MinGW-w64 所取代，且 MinGW 也已停止了更新。

因此，我们最终下载安装的是**MinGW-w64**

下载地址：

[mingw-w64sourceforge.net](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/mingw-w64/files/)

进去后往下滑，找到这个：

![img](https://pic4.zhimg.com/v2-2637308c782e35401f75f829d012f9a3_r.jpg)

下载下来后是一个压缩文件，将它解压缩(解压缩软件推荐[Bandizip](https://link.zhihu.com/?target=https%3A//www.bandisoft.com/bandizip/)）得到mingw64文件夹，然后把它拖动到一个合适的位置（或者直接解压缩到这个位置），地址中不要有中文，推荐C:\Program Files

![img](https://pic4.zhimg.com/v2-bb1db19a079938d04ab2e55fb4f6d993_r.jpg)

你可以打开bin目录看下，里面有很多后缀名是.exe 的可执行程序，这些就是开发时所需的工具，如：gcc.exe 是C语言程序的编译器，g++.exe 是C++语言的编译器，gdb.exe 是用来调试程序的 debug 工具。

还有一些头文件也里面，如stdio.h的位置是C:\Program Files\mingw64\x86_64-w64-mingw32\include

然后，为了让程序能访问到这些编译程序，需要把它们所在的**目录**（我这里是C:\Program File\mingw64\bin，点击地址栏进行复制）**添加到环境变量Path中**。

> 环境变量是 Windows 系统中用来指定运行环境的一些参数，它包含了关于系统及当前登录用户的环境信息字符串。当用户运行某些程序时，系统除了会在当前文件夹中寻找某些文件外，还会到环境参数的默认路径中去查找程序运行时所需要的系统文件。

用windows的搜索功能（快捷键是`Windows徽标键+S`）搜索`环境变量`，

![img](https://pic2.zhimg.com/v2-792e514f90cf94fd85b0e5b63b9e3f45_r.jpg)

打开它

![img](https://pic3.zhimg.com/v2-5b85a90688fc177926a59afc4a4aaeea_r.jpg)

![img](https://pic2.zhimg.com/v2-97f141744e6959b1460e5b1dd12ee8bd_r.jpg)

然后一路确定回去。

现在验证一下，搜索打开cmd命令提示符，输入gcc --version(中间有空格），按回车，看到如下信息 ：

![img](https://pic4.zhimg.com/v2-44062c26499bbf163384c3fd11898027_r.jpg)

说明gcc安装成功。

现在重启一下电脑。

好了，我们的电脑里已经有了这两个东西了，他们是从不同的地方下载的，安装的位置也不同，目前两者之间还没有任何联系，接下来，我们应该去搭建起他们之间的桥梁了，不过别着急，咱们先来了解一下文件结构。

## Step 2 文件结构

文件结构就是你组织文件夹、文件，决定他们怎样嵌套、怎样从属的方法。

这一步是区分搭建的是语言学习环境还是实际项目开发环境的关键。

这两者有什么区别呢？想想你写hello world时是怎样写的，你写了一个单文件，只有一个.c文件，然后你按下绿色三角进行编译运行生成.exe可执行文件，语言学习环境大都是这样的**单文件编译运行调试**，或者是涉及到**简单的几个头文件和源文件的组合**这样的多文件结构。而**实际项目开发**呢，实际中的一个小项目的目录结构可能长这样：

![img](https://pic1.zhimg.com/80/v2-42d3f9f7c84ec2d54a3047aac54c2a68_1440w.jpg)

我们的语言学习环境不是这样的，我们用不到lib、build、makefile等文件夹/文件，我们的目录结构应当方便我们新建一个单文件，然后编译调试，这些文件还应当在一起以方便查看和管理

具体怎么操作:

建议把代码都组织在一个地方，以方便管理。以我为例，我在C盘根目录建了一个名叫Codefield的文件夹，我所有代码相关的东西都组织在这里面。

现在，打开文件资源管理器，找一个合适的地方，创建一个这样的Codefield文件夹（文件夹的名字你也可以改成别的，注意路径中不要出现中文和空格，因为gcc调试器不支持中文路径），然后在这个文件夹下再新建一个文件夹CODE_C，你所有的c语言代码就放在这里面，由于vscode以文件夹组织项目，而我们涉及到单文件和简单的多文件两种情景，所以在CODE_C下再新建两个文件夹C_Single 和 C_Multiple ，这两个就是我们的工作区文件夹了。

![img](https://pic4.zhimg.com/v2-632c8c68fa9eb17273dd1d1194185813_r.jpg)

今后，涉及到其他代码相关的东西时，你就可以在Codefield文件夹下组织了，比如再学一门C++语言时，建一个CODE_Cpp文件夹；玩leetcode刷算法的时候，建一个Leetcode文件夹；从github克隆别人的项目时，建一个Github文件夹；自己做项目时，建个Projects文件夹……

现在，让我们看一下工作区文件夹，以C_Single为例，这其中的文件结构又该怎么组织？（这一步你不需要建文件，弄明白结构就好）首先要有一个.vscode文件夹(这是vscode的配置文件所在处，下一步会详细讲)，然后就是我们的源文件，在学习过程中，通常会写很多的源文件，把他们全堆在一起显然不够优雅，我们对这些源文件进行一下分类，比如按章节分：

![img](https://pic1.zhimg.com/80/v2-40426e7c008e47534f85478ad62d0274_1440w.jpg)

或者按类型分：

![img](https://pic4.zhimg.com/80/v2-7cefd4c8b789cb7db9366cbff4708d9b_1440w.jpg)

具体怎么分可以看你的学习情况。还有一个问题，源码编译后会生成exe可执行文件，它们放在哪里？和源文件放在一起的话，当文件夹下文件多起来时会非常杂乱，因此我们选择把exe文件统一放在bin文件夹下，这个bin文件夹不应当直接放在工作区文件夹下，这样会造成不同的源码分类文件夹下的文件都不能重名，于是我们在**每一个分类文件夹下**都建一个bin文件夹，最终效果如下：

![img](https://pic1.zhimg.com/80/v2-74ad14ad1177c5244763835ddb26be40_1440w.jpg)exercise目录下有个bin目录，hello.c在exercise下

C_Mutile类似但有所不同，由于一组程序由多个文件构成，我们把这C_Single中的单个源文件替换成文件夹就好，每个文件夹里面就是一组源文件，并且exe文件也放在其中，不需要单独的bin目录。

![img](https://pic3.zhimg.com/80/v2-bd7d320304336f46b4010193b5a414b2_1440w.jpg)

至此，你有了一个合适的文件结构，我们可以开始进行 vscode 的配置了。

## Step 3 vscode配置文件

这一步开始前，我们再来了解几个概念。

**命令行**：命令行 或 命令行界面，是一种基于文本的用来查看、处理、和操作计算机上的文件和程序的工具。

**终端/控制台**：普通用户可以简单的把终端和控制台理解为：可以输入命令行并显示程序运行过程中的信息以及程序运行结果的窗口。 不必要严格区分这两者的差别。

**shell**：终端自身并不执行用户输入的命令，它只是负责把输入的内容传送到主机系统，并把主机系统返回的结果呈现给用户。负责解释执行用户输入的命令并返回结果的，正是Shell，它是沟通用户和系统内核的中间桥梁。

现在思考一个问题，我们搭的这套环境中编辑器选的是vscode，但理论上任何能处理文本的编辑器都能用来写代码，比如Windows自带的记事本，你可以在桌面新建一个txt文件，命名为hello，然后用记事本写个helloworld程序进去，再把这个文件后缀改成.c，这就是一个源代码文件了，我们该如何对它进行编译运行呢？答案是**通过命令行**，我们已经安装了编译器套装并把它添加进了环境变量，现在可以使用gcc命令了：搜索打开cmd命令提示符，默认进入的是用户目录，输入`cd desktop` 进入桌面目录，像这样：

![img](https://pic1.zhimg.com/v2-363cd5eedb5f2da5c270db1db059d948_r.jpg)

然后输入编译命令 `gcc -o hello hello.c`（注意空格），按下回车，你会发现桌面多了hello.exe文件，这说明我们成功编译生成了可执行文件，然后再在命令行中输入`hello.exe`运行程序 。

这样每次都用命令行太麻烦了，我们希望用更快捷的方式执行这一过程，但记事本不是专门给你写代码的，它不能提供这样的配置，但是vscode就不一样了，专门写代码的编辑器当然有专门的方式让你快捷地编译运行。这是通过.vscode文件夹下的json配置文件实现的，这些json文件怎么写是由vscode开发团队规定的（感兴趣可以去看官方的文档），其中一个是tasks.json，task是任务的意思，我们的编译和运行就是我们想要vscode执行的任务，为此我们要在tasks.json里写两个task：`Build`和`Run`(这里为什么不是`Compile`呢？是因为从源码到可执行的过程中不仅是**编译(Compile)**,还有预编译、链接等过程，用**构建(Build)**来表述更合适)。除了编译和运行，我们还需要进行**调试(Debug)**，这个就不是通过task来实现的了，而是通过`launch.json`文件来实现。

现在，打开vscode，发现全是英文，我们先装个汉化插件：

![img](https://pic2.zhimg.com/v2-fd027deb175d8670a1bf42357be1cc85_r.jpg)

然后搜索C/C++安装这个插件，这是对语言的支持插件

![img](https://pic3.zhimg.com/v2-61b0d8504984927d6f9b287faba20a92_r.jpg)

重启vscode，打开C_Single文件夹：

注意要 文件->打开文件夹 这样打开，vscode中打开的**根目录**是C_Single

![img](https://pic2.zhimg.com/v2-2757e9a0cd4a66356d42be434f1c06dd_r.jpg)

然后新建`.vscode`文件夹(注意前面有个`.`),然后**在里面**新建`tasks.json`和`launch.json`

![img](https://pic1.zhimg.com/v2-b822c401134d6990133290d4e1ac3c68_r.jpg)打开的根目录是C_Single

下面是这两个文件的具体内容，带有详细注释，你要大致看一遍，看不太懂没关系。复制粘贴到你的文件里，注意里面有一些路径之类的东西需要你进行修改，还有一点是这里的配置和上一步中提到的工作区下的文件结构是严格一致的，必须那样组织文件。

**tasks.json**

```json
{
    "version": "2.0.0",
    "tasks": [
        {//这个大括号里是‘构建（build）’任务
            "label": "build", //任务名称，可以更改，不过不建议改
            "type": "shell", //任务类型，process是vsc把预定义变量和转义解析后直接全部传给command；shell相当于先打开shell再输入命令，所以args还会经过shell再解析一遍
            "command": "gcc", //编译命令，这里是gcc，编译c++的话换成g++
            "args": [    //方括号里是传给gcc命令的一系列参数，用于实现一些功能
                "${file}", //指定要编译的是当前文件
                "-o", //指定输出文件的路径和名称
                "${fileDirname}\\bin\\${fileBasenameNoExtension}.exe", //承接上一步的-o，让可执行文件输出到源码文件所在的文件夹下的bin文件夹内，并且让它的名字和源码文件相同
                "-g", //生成和调试有关的信息
                "-Wall", // 开启额外警告
                "-static-libgcc",  // 静态链接libgcc
                "-fexec-charset=GBK", // 生成的程序使用GBK编码，不加这一条会导致Win下输出中文乱码
                "-std=c11", // 语言标准，可根据自己的需要进行修改，写c++要换成c++的语言标准，比如c++11
            ],
            "group": {  //group表示‘组’，我们可以有很多的task，然后把他们放在一个‘组’里
                "kind": "build",//表示这一组任务类型是构建
                "isDefault": true//表示这个任务是当前这组任务中的默认任务
            },
            "presentation": { //执行这个任务时的一些其他设定
                "echo": true,//表示在执行任务时在终端要有输出
                "reveal": "always", //执行任务时是否跳转到终端面板，可以为always，silent，never
                "focus": false, //设为true后可以使执行task时焦点聚集在终端，但对编译来说，设为true没有意义，因为运行的时候才涉及到输入
                "panel": "new" //每次执行这个task时都新建一个终端面板，也可以设置为shared，共用一个面板，不过那样会出现‘任务将被终端重用’的提示，比较烦人
            },
            "problemMatcher": "$gcc" //捕捉编译时编译器在终端里显示的报错信息，将其显示在vscode的‘问题’面板里
        },
        {//这个大括号里是‘运行(run)’任务，一些设置与上面的构建任务性质相同
            "label": "run",
            "type": "shell",
            "dependsOn": "build", //任务依赖，因为要运行必须先构建，所以执行这个任务前必须先执行build任务，
            "command": "${fileDirname}\\bin\\${fileBasenameNoExtension}.exe", //执行exe文件，只需要指定这个exe文件在哪里就好
            "group": {
                "kind": "test", //这一组是‘测试’组，将run任务放在test组里方便我们用快捷键执行
                "isDefault": true
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true, //这个就设置为true了，运行任务后将焦点聚集到终端，方便进行输入
                "panel": "new"
            }
        }

    ]
}
```

**launch.json**

```json
{
    "version": "0.2.0",
    "configurations": [
        {//这个大括号里是我们的‘调试(Debug)’配置
            "name": "Debug", // 配置名称
            "type": "cppdbg", // 配置类型，cppdbg对应cpptools提供的调试功能；可以认为此处只能是cppdbg
            "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
            "program": "${fileDirname}\\bin\\${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
            "args": [], // 程序调试时传递给程序的命令行参数，这里设为空即可
            "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，相当于在main上打断点
            "cwd": "${fileDirname}", // 调试程序时的工作目录，此处为源码文件所在目录
            "environment": [], // 环境变量，这里设为空即可
            "externalConsole": false, // 为true时使用单独的cmd窗口，跳出小黑框；设为false则是用vscode的内置终端，建议用内置终端
            "internalConsoleOptions": "neverOpen", // 如果不设为neverOpen，调试时会跳到“调试控制台”选项卡，新手调试用不到
            "MIMode": "gdb", // 指定连接的调试器，gdb是minGW中的调试程序
            "miDebuggerPath": "C:\\Program Files\\mingw64\\bin\\gdb.exe", // 指定调试器所在路径，如果你的minGW装在别的地方，则要改成你自己的路径，注意间隔是\\
            "preLaunchTask": "build" // 调试开始前执行的任务，我们在调试前要编译构建。与tasks.json的label相对应，名字要一样
    }]
}
```

到这里，差不多就已经成功了，让我们写个简单的hello程序来试一下编译、运行、调试:

首先在C_Single下新建一个exercise文件夹，来组织源码文件，在exercise下新建`hello.c`文件，然后在exercise下建一个bin文件夹（注意从属关系，不要建错了，在vscode中想在某个目录下新建文件/文件夹要**先点击一下该目录，再点击新建按钮**），hello.c中输入如下代码：

```c
#include <stdio.h>
int main()
{
    char name[10];
    printf("Input your name: ");
    scanf("%s",name);
    printf("Hello,%s,this is your vscode!\n",name);
    return 0;
}
```

写好后`ctrl+s`保存，进行如下操作：

-   仅编译（构建），用快捷键`ctrl+shift+B`,你会发现终端面板打开了，显示如下：

![img](https://pic1.zhimg.com/v2-c1c1fbdf967946eb5d430731f5094084_r.jpg)

没有报错，bin文件夹下多了hello.exe，编译成功！

-   编译（构建）+运行，测试任务默认没有快捷键，我们自己绑定一个：点击左下角小齿轮->键盘快捷方式->搜索`任务`->找到`运行测试任务`,点击左侧加号添加键绑定，这里我们设为`F4`，

![img](https://pic4.zhimg.com/v2-0c093b3882150e97fecc1d4ff972cbef_r.jpg)

然后回到我们的hello程序页面，按下F4,显示如下：

![img](https://pic3.zhimg.com/v2-fe55781a28472dd6143a9d77abfc8b42_r.jpg)

输入你的名字，按下回车，运行成功！

-   接下来是调试（vscode的调试功能非常直观易用，你会爱上它的），在第一个printf处打上断点（点击行号前面的小红点，或者用快捷键`F9`)，然后打开左侧的运行面板，点击绿色小三角开始调试（或者直接用调试快捷键`F5`）

![img](https://pic1.zhimg.com/v2-73ceed3624c7e207f720b70b6e851cc4_r.jpg)

然后会出现调试工具栏，各按钮功能如图：

![img](https://pic3.zhimg.com/v2-1ce0a731f2c243facf67aa7a88a833ae_r.jpg)

左侧可以查看、监控变量

![img](https://pic3.zhimg.com/80/v2-abc726b66e49472982ed56f593598082_1440w.jpg)

我们使用单步调试按钮，快捷键`F11`,单步向下执行程序，黄色箭头所指示行是现在未执行、下一步将要执行的语句，当执行到输入语句时，黄色箭头会消失，这时你在终端面板内进行输入，然后按回车，黄色箭头重新出现，可以继续向下执行。

调试成功！

今后就可以新建源文件写程序，`F4`一键编译运行，`F5`一键开始调试

有没有很激动？ ^o^/

### 可能出现的问题

-   **中文乱码**

乱码问题是由于文件编码格式引起的，vscode默认的编码格式是UTF-8，而Windows的终端的默认编码是GBK，这就造成了中文会显示成乱码，解决办法是生成程序时指定用GBK，我们的task里已经指定了，所以理论上你不应该出现这个问题

-   **找不到头文件**

正确添加了环境变量的话，不应该出现这个问题

-   **“终端将被任务重用，按任意键关闭终端”**

按照我们在task中的设置，每次执行一个task就会打开一个新的终端面板，你可以在下拉列表查看自己打开的面板：

![img](https://pic3.zhimg.com/v2-9b7bf6a475f97025b9ce84539748faba_r.jpg)

如果你把task的"panle"属性改成了"shared",所有的任务都用的这一个终端，vscode会提醒你“终端将被任务重用”，这句话并不是报错，只是提醒，你无视它就好，而且在设置里还可以关闭这句提醒。

> 关于code runner
> 你可能在很多其他人的教程里见过它，这是一个第三方的插件，用它也可以实现编译运行，原理也是代替你手动输入命令行，也需要一定的配置。不过我觉得按照我的方法已经能很简单便捷地实现编译运行调试了，没必要再用这个插件。

### 简单的多文件程序

我们已经搞定了C_Single，多文件的C_Multiple的设置也类似，只需要改一下那两个配置文件涉及到路径的部分，文件如下，你可以对比一下：

**多文件tasks.json**

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "gcc", //写c++换成g++
            "args": [
                "${fileDirname}\\*.c", //写c++把 *.c 换成 *.cpp
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "-g",
                "-Wall",
                "-static-libgcc",
                "-fexec-charset=GBK",
                "-std=c11",  //写c++换成c++标准
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "new"
            },
            "problemMatcher": "$gcc"
        },
        {
            "label": "run",
            "type": "shell",
            "dependsOn": "build",
            "command": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "group": {
                "kind": "test",
                "isDefault": true
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": true,
                "panel": "new"
            }
        }

    ]
}
```

**多文件launch.json**

```json
{
    "version": "0.2.0",
    "configurations": [{
        "name": "Debug",
        "type": "cppdbg",
        "request": "launch",
        "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
        "args": [],
        "stopAtEntry": false,
        "cwd": "${fileDirname}",
        "environment": [],
        "externalConsole": false,
        "internalConsoleOptions": "neverOpen",
        "MIMode": "gdb",
        "miDebuggerPath": "C:\\Program Files\\mingw64\\bin\\gdb.exe",
        "preLaunchTask": "build"
    }]
}
```

两个文件夹有不同的配置，写单文件时就打开C_Single，写多文件时就打开C_Multiple，注意对应的文件结构。

有一点要注意，在写多文件时，包含自己写的头文件要用双引号，而不是尖括号，例如`#include "myHeader.h"`,双引号表示先在当前目录下寻找头文件。

你可以自己写个简单的多文件程序测试一下有没有问题。

## Step 4 更进一步

vscode的一大优点就在于插件生态丰富，通过插件可以扩展很多功能。这里推荐几个：

-   one dark pro

主题插件，好像是下载量最多的主题插件，整体配色比较和谐。（vscode在颜值方面真的很能打）

-   Material Icon Theme

一套精心设计的图标，可以让你的文件/文件夹更有辨识度

-   Code Time

可以多维度的记录你在vscode上花的时间，可以用这个插件记录你码代码的时间，比如你可以定个类似每天编程2小时之类的目标，督促激励自己学习编程。

-   Power Mode

这是一个炫酷的插件，可以给你敲代码的过程添加特效，效果炸裂，具体操作可以看插件详情页。

还有其他很多有用有趣的插件，你可以看看别人的推荐帖。

另外vscode还有其他很多功能，比如快捷键、小地图、搜索查找替换、代码片段、集成git等等，你可以慢慢探索。

**不过新手阶段，注意不要把时间全花在这些折腾上，工具只是工具，好好学习才是更重要的事，不要舍本逐末。**

## 结束语

我们的旅程结束了，幸运的话，你现在已经拥有了一个美妙的学习环境，你将vscode打磨成了一把趁手的兵器，它刀身优美、刀口锋利，打开，犹如战士拔刀对敌，关闭，犹如战士收刀入鞘，你拥有了在代码世界中劈荆斩棘的利刃，运用它、挥舞它吧。

编辑于 10-27

[Visual Studio Code](https://www.zhihu.com/topic/20017183)

[开发工具](https://www.zhihu.com/topic/19564417)

[代码编辑器](https://www.zhihu.com/topic/19852084)

赞同 1615247 条评论

分享

喜欢收藏申请转载

### 文章被以下专栏收录

-   [一个计算机小白的心路历程](https://www.zhihu.com/column/c_1226480255013474304)

[心中的余火]
在调试的时候出现Unable to start debugging，怎么解决啊😭

-   [冷却]
    检查一下环境变量和两个json文件，还不能解决可以给我私信

-   [Haise]

    真要是小白，我觉得下vs就行。下载，安装，写代码，无需任何配置

-   [慢慢]

    除了大了一点 其他都比vscode友好多了 配置环境一弄三个小时 还失败当场去世

-   [一笑琅然]
    请问，多文件编译出现了这个错误，该怎么解决
    g++.exe: error:d\Codefield\CODE_Cpp\Cpp_Muliple\helloworld*.c: Invalid argument
    g++.exe: fatal error: no input files
    compilation terminated.

-   [冷却]

    _.c改成_.cpp

-   [Yui]
    请问，我在code_single文件夹下写了个cpp程序，编译的时候cc1plus. exe: warning: command line option '-std=c11' is valid for C/ObjC but not for C++, 这个是需要改task. json 吗

    c11是c语言标准，换成c++的就好了
