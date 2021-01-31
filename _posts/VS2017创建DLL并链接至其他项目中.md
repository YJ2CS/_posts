---
title: VS2017创建DLL并链接至其他项目中
url: VS2017创建DLL并链接至其他项目中
author: YJ2CS
avatar: /custom/avatar.webp
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - IDE
tags:
  - IDE
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-827'''
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T20:03:57+08:00'

---

# VS2017创建DLL并链接至其他项目中【转】 - cynophile的博客

已剪辑自: <https://blog.csdn.net/cynophile/article/details/79749524>

1. 启动VS2017，点击菜单栏上的“文件->新建->项目”创建一个新的开发项目；

![](9ab97110.png)

2. 在弹出的“新建项目窗口”中，选择左侧“Visual C++”列表下的“Windows桌面”，然后选择右侧的项目类型为“动态链接库（DLL）”，接着设置项目名称和存储位置以及解决方案名称。配置完毕后，点击“确定按钮”确定创建动态链接库项目；

![](3d7ac0a2.png)

该步骤之后，文件夹中生成DLL1文件夹，包含如下内容
![](84711d44.png)

3. 项目创建之后，点击VS2017界面菜单栏上的“生成 > 生成解决方案”编译新创建的项目代码，确认是否存在问题（极少会出现问题）；

![](1c630ddb.png)

生成解决方案后，会在Debug文件夹中生成如下文件
![](1827d457.png)

4. 编译结束之后，可以在VS2017的输出窗口中见到编译成功的输出信息；

![](7adc3944.png)

5. 在VS2017开发界面中，右键单击“解决方案”里面“Dll1”项目下的“头文件”目录，在弹出菜单中选择“添加 > 新建项”；

![](d4bb4a97.png)

6. 在弹出的“添加新项”对话框中，选择“头文件(.h)”，然后输入头文件的名称“dll1.h”，之后点击“添加按钮”确定添加一个名为“dll1.h”的头文件；

![](b034c42b.png)

相应文件中多处dll1.h头文件
![](d0d20741.png)

7. 在Windows中，定义在dll中的变量、函数和类，如果希望让别的程序能够访问。必须通过manifest文件指定导出目标（变量、函数或类）或者通过_declspec(dllimport)关键字指定需要导出的目标，然后在使用dll的程序中通过_declspec(dllimport)关键字指定导入的目标。在开发中使用_declspec()定义导出/导入目标是最方便的做法，因此，可以继续向“dll1项目”中添加一个头文件 “export.h”，然后添加自适应导出/导入目标的宏；（图中第一个应该为_declspec(dllimport)第二个为_declspec(dllexport)，存在图中错误）

![](c064f29d.png)

文件中反应出来的是，在如下目录下生成export.h文件
![](5890b4ae.png)

8. 鼠标选中DLL1项目右键“ 属性”，打开Dll1项目的属性页窗口；

![](6702f416.png)

9. 在弹出的“Dll1属性页窗口”中，将配置设置为”所有配置”,然后选中“C/C++ > 预处理器”，接着在“预处理器定义”右侧的属性值中增加“EXPORT_DLL”。设置完毕后，点击“确定按钮”确定属性设置；

![](65cbec55.png)

10. 在属性页中定义了EXPORT_DLL宏之后，export.h文件中EXPORT_API宏对应的值就变成了__declspec(dllexport)，对于Dll1项目而言，只要使用EXPORT_API修饰的对象，都将变成导出目标。相对而言，在引用Dll1的另一项目中，默认是没有定义EXPORT_DLL宏的，那么用EXPORT_API修饰的对象，则都是导入目标；

![](4e0b9e6e.png)

11. 打开 “dll1.h”文件，使用#include包含“export.h”头文件，然后使用EXPORT_API声明一个名为printHello()的DLL导出函数；

![](898e698c.png)

12. 打开“Dll1.cpp”，包含“stdio.h”头文件并写入printHello()函数的实现；

![](bb7dad13.png)

13. 生成解决方案(F7)，可以在输出窗口中见到所有代码均编译成功；

![](70fcf25f.png)

相应的在Debug文件夹下，多处如下几个文件
![](0c1c2dc8.png)

14. 右键单击左侧列表中“解决方案”，然后在弹出菜单中选择“添加 > 新建项目”，向解决方案中添加一个新的控制台项目，用于测试Dll1中导出的printHello()函数是否可以正常访问；

![](bc79fc66.png)

15. 在弹出的“添加新项目窗口”中，选择“Windows桌面 > Windows控制台应用程序”，然后输入新项目的名称并点击“确定按钮”创建新项目；

![](78a561b4.png)

相应文件中表现如下
![](13fde3ed.png)

包含文件如下
![](a99bb751.png)

16. 右键单击“解决方案”列表中新创建的控制台项目，在弹出菜单中选择“设置为启动项目”，将控制台项目设置为启动项目。这样，每当运行程序时，启动的就是这个新创建的控制台项目（DLL项目是无法独立运行的）；

![](4228fbb4.png)

17. 点击VS2017界面中的“本地Windows调试器”按钮，编译并运行控制台项目，会发现有一个黑屏幕一闪而过。这是由于控制台程序在执行完main()函数后，直接退出了（DevCPP中则会自动暂停，防止控制台直接退出）。

![](9055f401.png)

文件中反应如下
![](727e79d0.png)

18. 为了让控制台程序执行完毕后依然保持开启状态，可以打开 ConsoleApplication1.cpp文件，然后包含stdlib.h文件并在main()函数体的return语句之上添加程序暂停语句”system(“pause”);”

![](309d8bd2.png)

19. 写好代码之后，再次调试运行程序，可以见到控制出现并暂停。在控制台窗口中，点任意键可以退出控制台程序；

![](97cd6e93.png)

20. 回到VS2017开发界面，右键单击“解决方案管理器”列表中的控制台项目，在弹出菜单中选择“生成依赖项 > 项目依赖项”打开“项目依赖项窗口”；

![](6ab27913.png)

21. 在弹出的“项目依赖项窗口”中，勾选依赖于“Dll1”，表示控制台项目ConsoleApplication1依赖Dll1项目。这样可以保证每次编译ConsoleApplication1时，编译器总会自动先编译Dll1项目，保证Dll1总是最新的。设置完毕后，点击“确定按钮”关闭“项目依赖项窗口”；

![](2bd96a65.png)

22. 右键单击VS2017工作区中的“Dll1.cpp选项卡页”，在弹出菜单中选择“打开所在的文件夹”；

![](a2cd8254.png)

23. 在打开的Dll1.cpp文件所在的文件夹中，点击返回按钮，重新进入到Dll1项目的Debug输出目录中。在该目录中可以见到Dll1项目生成的符号链接库“Dll1.lib”和动态链接库“Dll1.dll”。 如果需要在另一个项目中加载“Dll1.dll”文件，那么通过链接“Dll1.lib”是最简便的方式（否则就要通过LoadLibrary()及相关函数通过代码加载动态库了）；

![](0ebaede2.png)

24. 返回VS2017开发界面中，右键单击“解决方案列表”中的ConsoleApplication1，在弹出菜单中，选择“属性页”，打开控制台项目的属性页；

![](773e92e4.png)

25. 在弹出的ConsoleApplication1属性页窗口中，将配置设置为“所有配置”，然后在左侧“配置属性”列表中，选择“链接器 > 常规”，接着在右侧属性列表中选择“附加库目录”属性右方的编辑框，在弹出的下拉列表中选择“编辑”；

![](544a51b6.png)

26. 在弹出的“附加库目录窗口“中，点击”宏(M) >>“按钮，展开VS2017的宏列表；

![](26549a3c.png)

27. ![](0edbd76a.png)

28. 返回“附加库目录窗口”中，点击“新建文件夹图标”，然后在新出现的附加目录项中填入“$(OutDir)”并点击”确定按钮”结束附加库设置；

![](bd7f70ed.png)

29. 附加库设置完毕后，可以在项目属性页中见到“附加库目录”属性右方已经被填入了设置的值；

![](ba1f1c94.png)

30. 选择“ConsoleApplication1属性页”左侧列表中的“输入”，然后在右侧“附加依赖项”中填入“dll1.lib;”，告诉编译器需要链接dll1.lib，进而加载我们需要的“Dll1.dll”。设置之后，点击“确定按钮”关闭属性页；

![](1e88084a.png)

31. 在VS2017工作区中，打开“ConsoleApplication1.cpp”文件，然后在代码中包含“dll1.h”（注意这里的相对路径，目录起点为ConsoleApplication1.cpp所在的目录），之后在main()函数中添加调用printHello()函数的代码；

![](8352bd0f.png)

32. 生成解决方案（或F7），一切正常时，可以在VS2017输出窗口中见到编译成功的输出信息。如果出错，则根据提示修改项目配置或代码后重新生成解决方案，直到成功为止；

![](c6ba7fb1.png)

相应文件更新为
![](021c36bd.png)

33. 运行ConsoleApplication1程序，在弹出的控制台界面中可以见到输出的“Hello”字符串，表示Dll开发成功。Enjoy！

![](b29e6cb6.png)
![](6ff6dee6.png)

VS2017删除右键菜单“在Visual Studio中打开”-百度经验
已剪辑自: <https://jingyan.baidu.com/article/60ccbceb4e3f0e64cab19737.html>
Visual Studio 2017 下载安装后会在系统桌面或文件夹中出现一个右键的菜单“在 Visual Studio 中打开”，可能有人不太喜欢这个，此时你可以尝试下这篇经验。这里将逐步为你演示这些操作。关键还是得从注册表入手。

vs2017 找不到源文件stdio.h解决方法 - fmbao的博客 - CSDN博客
已剪辑自: <https://blog.csdn.net/u011268787/article/details/80784491>
这个问题网上又不少人提出。我的vs出现这个问题是因为我电脑重装系统了，原来的项目所采用windows SDK 已经发生了变化。因此解决的办法是：项目->属性->配置属性->常规->windows SDK版本。将其换成你现在的版本即可解决问题。

VS2017 C++ 无法打开源文件: “SDKDDKVer.h”, "stdio.h", "tchar.h" - xapxxf的博客 - CSDN博客
已剪辑自: <https://blog.csdn.net/xapxxf/article/details/78356612>
第一次安装好就出现了图片中这个错误。为此搜索了好久，以为是安装问题，卸载重装好几次……30G的东西，真不是闹着玩的。
![](efeac73b.png)

最后却发现，这个问题的原因是！！！在Visual Studio的勾选 默认安装的情况下，有文件默认是没有勾选的，也就是默认不安装的。（也应该是因为Win10SDK安装失败，不过怎么装也装不上，就算了。。。
![](d1c4b79c.png)
![](dba40efc.png)

，导致 出现的问题）
而！！SDKDDKVer.h和stdio.h等正是存在于这些文件中。
默认不安装，如图：
![](629b47ba.png)

由于自己被这个坑了很久，想着网上一搜全是各种单独下载，后者搜索，然后再重新配置等等无比麻烦的操作。来此分享一下，最为简单且一劳永逸的方法

解决方法：
只需要再程序和功能中，找到VS，右键，修改，再把那几个勾选上，就好了。
![](d085f011.png)

右键，选择-更改
然后可以看到：
![](62e7f259.png)

然后，点击更多，选择-修改（照着勾吧。。后面补的图，发现自己忘了那几个是未解决问题勾的了）
![](90bdf63c.png)

找到C++的，勾上，再确认修改就OK，等待安装完，问题解决。完全不用自己再去下载WindowsSDK那么麻烦。

解决！
（这之后就可以用了，我的开发需求就基本能满足，但是这是安装Win10SDK失败的情况下，的无奈之举了）

自己单独安装的话，可以借鉴这一篇文章：http://blog.csdn.net/hhh1108/article/details/50352027
也有一点好处，这样可以安装到其它盘了。自己配置一下。VS会安装到C盘。

当然了，如果你电脑能正常安装Win10SDK，那么就该没这种问题了。

VS2017安装完成之后无法找到源文件windows.h，stdio.h等头文件的问题解决办法 - z_m_1的博客 - CSDN博客
已剪辑自: <https://blog.csdn.net/z_m_1/article/details/80833782>
1.问题描述：
Visual Studio 2017安装完成之后，在源码中提示：
“无法找到源文件windows.h”
“无法找到源文件stdio.h”
“无法找到源文件tchar.h”
报错代码如下所示：
![](e2fba886.png)

![](3f030c2b.png)
2. 问题产生原因：
是由于在安装vs2017的过程中，少选了“用于桌面 C++ [x86 和 x64]的 Windows 10 SDK (10.0.16299.0)”这个模块。
3.问题解决：
（1）打开VisualStudio Installer，在Visual Studio Installer中点击修改；
（2）选择单个组件；
（3）勾选“用于桌面 C++ [x86 和 x64]的 Windows 10 SDK (10.0.16299.0)”模块，勾选此模块的时候会自动添加“用于 UWP (C++)的 Windows 10SDK (10.0.16299.0)”和“用于 UWP(C#、VB、JS)的 Windows 10 SDK (10.0.16299.0)”模块；
（4）最后点击修改按钮，添加这些模块。问题就解决了。
4.操作过程如下图所示：
![](020fdd00.png)
![](8ed091c7.png)
![](080e820d.png)

![](4f62083d.png)

VS下使用VIM， Visual Studio 安装 VSvim插件 配置 及使用 - QIYUEXIN - 博客园

已剪辑自: <https://www.cnblogs.com/qiyuexin/p/10424755.html#_label2>

正文
回到顶部
简介
VIM是一款很高效的编辑工具，所幸的是VS2012以后支持VIM的插件：VsVim。下面介绍插件的安装、配置及简单使用。
回到顶部

1. 下载安装

去官网下载，双击直接安装后，重新打开VS。
<https://marketplace.visualstudio.com/items?itemName=JaredParMSFT.VsVim>
安装完成后是这个样子的：
![](e3aedfd2.png)
会提示快捷键冲突，下面介绍相关配置。

回到顶部
2. 插件配置
2.1 关闭编辑框
Tools -> options：
![](559defbd.png)

这时信息会在屏幕的最下方显示：

![](65e41950.png)
可以在 View->OtherWindow->Command Window 中，打开命令窗口（ctrl + alt + a）：
![](4c4c5dda.png)

2.2 快捷键配置
Vim的快捷键与VS的快捷键有很多冲突，这里我仅把自己常用的快捷键改了过来：
![](4dafb1d7.png)
![](4b7bb3f3.png)
2.3  VsVim配置文件
vs中所有可以设置快捷键的命令，都可以被调用。
查看命令：在vs中，选择工具->选项->环境->键盘，
使用英文版vs,命令一目了然，每行都是一个命令，都可以被调用：

使用命令:set可以查看_vimrc的存放路径，一般为：C:\Users\Administrator，在该目录下新建文件“_vimrc”没有后缀名，写入如下内容：

" 1. 注释
:vnoremap ci :s/^///<cr>
:vnoremap cu :s////<cr>
:nnoremap ci :s/^///<cr>
:nnoremap cu :s////<cr>

" 2.相关配置
" 单个文件中：
":noremap gd <c-]>zz "跳转到定义"
:nnoremap gc :vsc Build.Compile "编译"
:nnoremap gb :vsc Build.BuildSolution   "build the solution"
:nnoremap gs :vsc Debug.StopDebugging   "结束调试"
:nnoremap gr :vsc Debug.Start   "开始调试"

":vnoremap gq ==
":nnoremap <space> za "折叠"
:nnoremap zm :vsc VAssistX.ListMethodsInCurrentFile<cr> "函数列表"
:nnoremap cj :vsc VAssistX.FindReferencesinFile<CR> "当前文件中的引用"
:nnoremap ca :vsc VAssistX.FindReferences<CR> "查看所有引用"
:nnoremap cm :vsc File.OpenContainingFolder<CR> "打开所在文件夹"
:nnoremap zj :vsc Edit.QuickInfo<CR> "查看函数定义文档"
:nnoremap zp :vsc VAssistX.RefactorImplementInterface<CR> "实现接口"

"visual模式中的查找"
:vnoremap * "/y/<C-r>/<CR>
:vnoremap # "/y?<C-r>/<CR>

.多文件
:nnoremap <c-o> :vsc View.NavigateBackward<CR>\
:nnoremap <c-i> :vsc View.NavigateForward<CR>

"打开查看类的对话框
:nnoremap cs :vsc VAssistX.FindSymbolDialog<CR>

"打开查看文件的对话框
:nnoremap cf :vsc VAssistX.OpenFileInSolutionDialog<CR>

"open VAOutline
:nnoremap co :vsc VAssistX.VAOutline<CR>

"打开解决方案资源管理器
:nnoremap cv :vsc View.SolutionExplorer<CR>

"在文件中查找
:nnoremap ck :vsc Edit.FindinFiles<CR>

重启VS。
