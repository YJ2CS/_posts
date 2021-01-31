---
title: VsVim安装配置
url: vsvim安装配置
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - Vim
tags:
  - VsVim
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-4397'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-26T11:46:41+08:00'

---

VsVim和一般意义上的vim还有些不同，具体来说，vsvim受制于VS对编辑器的种种限制，实际上是

使用vimscript编写的

完成已有vim插件部分功能的

一个Vs平台的插件

初学者大可不必在Vim上浪费时间，这部分内容放到你学习Linux系统时的后期，应当是比较合理的安排
安利visual studio最好用的vim插件vsvim。
1.安装

在vs中工具->扩展和更新->联机，搜索vsvim，创建者是Jared Parsons.
ps. VsVim是开源的，在github上有源码。作者维护的挺勤的。

2.配置文件

所有xxVim的配置都有一个配置文件，在linux下点开头.xxvimrc，在pc中是下划线开头_xxvimrc。
创建vsvim的配置文件，命名为_vsvimrc，放到C:\Users\“你的pc名称”下。比如，我的配置文件放到了 C:\Users\myPC 路径下。\
其他工具的vim插件，配置文件也是这样命名和放置。

3.vistual studio的命令调用

安装好vsvim之后，如果想使用vs中的命令，在命令模式下，调用的格式为

:vsc xxxxxxx

例如，想要调用查找光标下字符的所有引用，可以使用:

:vsc Edit.FindAllReferences

vs中所有可以设置快捷键的命令，都可以被调用。
查看命令：在vs中，选择工具->选项->环境->键盘，
使用英文版vs,命令一目了然，每行都是一个命令，都可以被调用：
这里写图片描述
<https://img-blog.csdn.net/20171213132817748?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWFuY2hlenVv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast>
其他插件可以调用的命令，也在这里可以查找到。

4.在_vsvimrc中配置快捷调用，直接贴一下我的配置文件：

set ai
"自动缩进宽度
set sw=4
set ts=4
"关闭高亮显示
set nohlsearch\
set is
set ignorecase

set backspace=indent,eol,start
set clipboard=unnamed
set number

:nnoremap . .<Esc>

"重新生成选中项目的解决方案快捷键
:nnoremap ,b :vsc Build.RebuildSelection<CR>

:inoremap jj <esc>
:nnoremap <c-j> <c-w>j
:nnoremap <c-k> <c-w>k
:nnoremap <c-l> <c-w>l
:nnoremap <c-h> <c-w>h

:nnoremap <c-o> :vsc View.NavigateBackward<CR>
:nnoremap <c-i> :vsc View.NavigateForward<CR>

:nnoremap ,t :vsc Window.NextTab<CR>
:nnoremap ,r :vsc Window.PreviousTab<CR>

:nnoremap ,n :vsc Window.NextDocumentWindow<CR>
:nnoremap ,p :vsc Window.PreviousDocumentWindow<CR>

"显示错误列表的快捷键
:nnoremap cl :vsc View.ErrorList<CR>
:nnoremap cn :vsc View.NextError<CR>
:nnoremap cp :vsc View.PreviousError<CR>

"添加注释
"vv是为了退出visual line模式
:vnoremap ci :s/^///<cr>vv
:vnoremap cu :s////<cr>vv
:nnoremap ci :s/^///<cr>
:nnoremap cu :s////<cr>

"居中显示查找结果
:nnoremap n nzz
:nnoremap N Nzz
:nnoremap * *zz
:nnoremap # #zz
"去定义
:nnoremap gd <C-]>zz

"gq->== 整理代码格式
:nnoremap gq ==
:vnoremap gq ==

"format code
:nnoremap == :vsc Edit.FormatDocument<CR>

"重命名
:nnoremap gr :vsc VAssistX.RefactorRename<CR>

"查看函数列表 list methods
:nnoremap zm :vsc VAssistX.ListMethodsInCurrentFile<CR>

"查找所有引用--
:nnoremap ca :vsc Edit.FindAllReferences<CR>
"或者使用VA的命令（vs2017中使用va命令比较好）
" :nnoremap ca :vsc VAssistX.FindReferences<CR>

"打开查看类的对话框
:nnoremap cs :vsc VAssistX.FindSymbolDialog<CR>

"打开查看文件的对话框
:nnoremap cf :vsc VAssistX.OpenFileInSolutionDialog<CR>

"open VAOutline
:nnoremap co :vsc VAssistX.VAOutline<CR>

"打开解决方案资源管理器
:nnoremap cv :vsc View.SolutionExplorer<CR>

"查找在当前文件中的引用
:nnoremap cj :vsc VAssistX.FindReferencesinFile<CR>

"在文件中查找
:nnoremap ck :vsc Edit.FindinFiles<CR>

"打开文件所在文件夹
:nnoremap cm :vsc File.OpenContainingFolder<CR>

"快速查看方法定义
:nnoremap zj :vsc Edit.QuickInfo<CR>

"快速查看方法的所有定义,鼠标在方法parameter上的时候显示的东西
:nnoremap zk :vsc Edit.ParameterInfo<CR>

"打开查找符号结果
:nnoremap zs :vsc View.FindSymbolResults<CR>

"打开查找结果1
:nnoremap zi :vsc View.FindResults1<CR>

"打开查找结果2
:nnoremap zu :vsc View.FindResults2<CR>

"打开va的在本文件中查找结果
:nnoremap ,i :vsc VAssistX.FindReferencesResults<CR>

"实现interface接口
:nnoremap zp :vsc VAssistX.RefactorImplementInterface<CR>

"可视模式中，使用 * 和 # 查找
:vnoremap * "/y/<C-r>/<CR>
:vnoremap # "/y?<C-r>/<CR>

"打开折叠或者关闭折叠
:nnoremap <space> za
