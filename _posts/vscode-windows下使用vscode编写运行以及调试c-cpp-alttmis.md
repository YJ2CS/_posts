---
title: Vscode windows下使用vscode编写运行以及调试C/C++ - Alttmis
url: vscode-windows下使用vscode编写运行以及调试c-cpp-alttmis
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
no-photos: >-
  https://random.52ecy.cn/randbg.php?size=1&rid-6e58
date: 2020-11-10 22:16:00
---


# Vscode windows下使用vscode编写运行以及调试C/C++ - Alttmis
已剪辑自: https://www.cnblogs.com/TAMING/p/8560253.html
不要转载，唯一出处：tangming博客园
最后更新于2019年8月12日：
本文原本为我在一年多前在参加算法竞赛期间于博客园发布的一篇整理vscode编写c/c++全部使用心得的文章，经过多次的修改/订正/完善受收到了很多朋友的关注和支持，很感谢大家，但这篇文章经历多次修改和添加，冗长并且混乱，我希望能重新组织一篇更加优质的文章并使用更适合于初学者的演示。而博客园在一些功能上存在一定局限，因此我重新在知乎上另起了一篇更加美观和易于阅读的文章
 
新文章的地址是：https://zhuanlan.zhihu.com/p/77645306
-----------------------------下面是原文章---------------------------------
vscdoe是一款稍有研究就会为之惊叹的软件
vscode支持类似于vs的断点调试c/c++，也可以直接编译&运行c/c++
 
先是编译运行 c/c++的方法                             
 
微软官方起初设定的科学做法(这也是现在的科学做法)是通过在vscode集成控制台写命令行的方式来实现编译运行程序的,但也可以通过code runner插件来简化步骤,实现一键编译执行
但无论是什么方法,因为vscode本身并不带有编译器,都需要自己提前安装好一个c/c++编译器(如mingw,clang)并且配置好环境变量(不会请点击这里)
控制台下编译运行C/C++(如果不懂命令行操作可以暂时跳过这里):
按 ctrl + ~ 打开vscode控制台,点击终端,在vscode的终端下操作其实就是在windows下的cmd或者powershell下操作,一切的编译运行等操作可以用输入命令行的方式来实现,只要掌握各自的编译器的命令行指令就能让程序在vscode界面上运行起来
c/c++编译器的那一套自然不在话下(如下图),先用cd 命令切换到源文件目录或者直接输入完整路径名,然后用编译器指令(假设编译器是mingw) g++ xxx.cpp -o xxx.exe编译,接着再输入./xxx.exe就可以运行编译好的程序
其他的c/c++编译器如clang包括其他语言(Python ,Go,Java...)都可以类似的这样操作

便捷方式是使用code runner插件:
code runner插件默认的c/c++编译器是gcc/g++,需要提前安装好并且设置好环境变量，通常选择MinGW或者MinGW-w64，建议选mingw-w64，一般用户建议下载离线版解压后添加环境变量，离线版下载地址：链接
不会请看:安装mingw-w64具体过程
同时,code runner插件使用的编译器是可以被修改为gcc/g++以外的编译器的(比如clang,MSVC),有这方面需要请看:vscode修改code runner使用的编译器
mingw和mingw-w64是有区别的，直观的说,mingw-w64更加强大
安装好并且设置好二者中的一个,并设置好环境变量后在cmd下输入gcc -v确认是否成功,出现关于gcc -v的相关信息(如下图)就表示成功

接着点击vscode左侧面板中的插件商店按钮
安装好下面两个插件
C/C++
Code runner
如果需要中文请安装一个chinese插件

注意,如果没安装clang的话不要安装推荐插件里的c/c++ clang插件,否则应该会报错
 
安装好后重启一下vscode这样就能在右上角看见一个三角形了，打开文件点击就能编译执行
但此时会有这样一个问题
如果程序里有scanf()等请求键盘输入数据的函数，此时无法从键盘输入数据，并且程序无法结束需要关闭重启vscode才能重新执行

解决办法是依次打开：文件>首选项>设置>用户设置>拓展>Run Code Configuration
找到  Run In Terminal  打上勾 这样运行的程序就会运行在vscode的集成控制台上
在工作区设置也有这个选项，但工作区设置只会对工作区生效
这样问题就能解决了

 运行一段测试代码

#include<bits/stdc++.h>  
using namespace std;  
  
int main(){  
    cout<<"hello"<<endl;  
    int u;  
    while(cin>>u){  
        cout<<u*u<<endl;  
    }  
}  

这时输出信息会显示在终端栏下面
随便输入测试数据
可以看到下图的效果
 

点击右上角的垃圾桶能提前结束程序运行
code runner插件有一个局限,code runner插件的原理其实是自动在控制台下帮助我们输入g++ xxx.cpp -o xxx.exe(假设是默认情况)这条编译指令,不会再添加额外的命令,比如如果代码中使用了winsock2用g++编译的话需要额外添加-lwsock32指令,即完整指令为g++ xxx.cpp -o xxx.exe -lwsock32,此时直接使用code runner的话会无法编译,这种情况应该使用上面提到的vscode集成控制台手动输入编译指令编译
 
调试 c/c++方法          
首先一点：不支持中文路径！！！（文件名和整个文件路径名中都不能有中文，否则无法调试，是由mingw不支持中文路径造成的）
实际效果类似vs那样按f5断点调试
首先选中一个用于存放各种代码的文件夹作为根路径也就是工作区，因为调试只会对根路径下的文件生效！！！
在vscode中打开这个文件夹（文件>打开文件夹>选中你的文件夹）
之后再在这个文件夹新建一个 .vscode 的文件夹，不要忘了开头的 "." 号(如果已有则不必再额外新建)
再在.vscode文件夹中新建两个配置文件 launch.json 和 tasks.json
类似于下图

之后再把下面的两个段代码粘贴到对应的文件里
这里需要修改一处：launch中 "miDebuggerPath" 选项需要设置为你的调试器(gdb.exe)所在位置 这里的是我电脑上MinGW -w64的安装位置
无论安装的是MinGW还是mingw-w64，都会有一个gdb.exe在安装目录的bin文件夹下，一定要把对应的路径修正否则无法调试
launch.json
tasks.json
之后打开在当前工作区子目录下的.c/cpp文件就可以添加断点进行调试了
此时可以按 ctrl+shift+b 直接调用配置好的g++ task 编译程序而不运行程序，类似于一些IDE的编译选项
如果我们要查看当前某个变量的值或者某个表达式的值,可以像vs一样在左侧的调试面板添加监视

也可以在下方的调试控制台里直接输入表达式或者变量名

当然,最简单的还是鼠标直接移动到变量上,往往直接就显示出来了,如果靠这样不能解决的话,就试试上面两种方法

vscode支持实时报错,遇到找不到头文件的问题请点击 

也可以让c/c++程序的调试在vscode的集成控制台上进行,不在额外显示黑窗口,类似于code runner的界面效果
只需将launch.json中的 "externalConsole" 项由 true 改为 false 
经评论区提醒
此时可能会遇到这样一个问题，如果你的输入法当前是中文输入的话，输入数据时会很长时间才能反应过来，只需要按shift将输入法切换到英文状态就不会遇到这个问题了，可以直接设置输入法首选项为英文

效果

类似于code runner的问题: 如果是需要有额外的编译指令如-lwsock32,需要调试前事先在tasks.json的args处添加上对应的指令,或者用 // 注释掉launch.json中的 preLaunchTask:"g++"(启动调试前执行g++编译按tasks指令格式编译) 这一项,然后自己在按ctrl + ~ 打开终端手动编译好后再执行调试
 
记住：调试是属于工作区设置，当前配置的调试环境只会对当前.vscode文件夹所在路径下的文件生效，如果要换用别的文件夹，把.vscode这个文件夹拷贝过去即可
 
最后,我使用的主题插件为tangming Themes,感兴趣的可以去插件商店下载,里面一共四个主题
 
似乎有很多打ACM的同学在看，再提醒一点，在上面提到集成终端下调试，将题目测试数据粘贴到命令行，测试到一半就发现问题，点重新启程调试的按钮，会因为剩余的数据未被读取而造成错误
比如这样的错误信息：
所在位置 行:1 字符: 2
+ 5& 'c:\Users\tangm\.vscode\extensions\ms-vscode.cpptools-0.24.1\debug ...
+  ~
表达式或语句中包含意外的标记“&”。
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : UnexpectedToken
正常现象，再重新点一下启动就可以了

Vscode Visual Studio代码显示语言（区域设置）
已剪辑自: https://code.visualstudio.com/docs/getstarted/locales
显示语言
默认情况下，Visual Studio Code附带英语作为显示语言，而其他语言则依赖于Marketplace提供的语言包扩展。
VS Code检测到操作系统的UI语言，并会提示您安装适当的语言包（如果在Marketplace上可用）。以下是推荐简体中文语言包的示例：

安装语言包扩展名并按照提示重新启动后，VS Code将使用与您的操作系统的UI语言匹配的语言包。
更改显示语言#
您还可以通过使用“ 配置显示语言”命令显式设置VS Code显示语言来覆盖默认的UI语言。
按Ctrl + Shift + P调出命令面板，然后开始键入“显示”以过滤并显示“ 配置显示语言”命令。

Press Enter and a list of installed languages by locale is displayed, with the current locale highlighted.

Use the "Install additional languages..." option to install more Language Packs from the Marketplace, or select a different locale from the list. Changing the locale requires a restart of VS Code. You will be prompted to restart when you select a locale.
The Configure Display Language command creates a locale.json file in your user VS Code folder. Depending on your platform, the locale.json file is located here:
	• Windows %APPDATA%\Code\User\locale.json
	• macOS $HOME/Library/Application Support/Code/User/locale.json
	• Linux $HOME/.config/Code/User/locale.json
The locale can also be changed by editing this file and restarting VS Code.
Available locales#
Display Language Locale 
	
English (US)	en
Simplified Chinese	zh-CN
Traditional Chinese	zh-TW
French	fr
German	de
Italian	it
Spanish	es
Japanese	ja
Korean	ko
Russian	ru
Bulgarian	bg
Hungarian	hu
Portuguese (Brazil)	pt-br
Turkish	tr
Marketplace Language Packs#
As described above, VS Code ships with English as the default display language, but other languages are available through Marketplace Language Packs.
You can search for Language Packs in the Extensions view (Ctrl+Shift+X) by typing the language you are looking for along with category:"Language Packs".

You can have multiple Language Packs installed and select the current display language with the Configure Display Language command.
Setting the Language#
If you want to use a specific language for a VS Code session, you can use the command-line switch --locale to specify a locale when you launch VS Code.
Below is an example of using the --locale command-line switch to set the VS Code display language to French:
code . --locale=fr 
Note: You must have the appropriate Language Pack installed for the language you specify with the command-line switch. If the matching Language Pack is not installed, VS Code will display English.
Common questions#
Unable to write to file because the file is dirty#
This notification may mean that your locale.json file wasn't saved after a previous change. Make sure the file is saved and try to install the Language Pack again.
Can I contribute to a language pack's translations?#
是的，Visual Studio代码社区本地化项目对任何人开放，供稿人可以在其中提供新翻译，对现有翻译进行投票或提出流程改进建议。
2019年9月4日
