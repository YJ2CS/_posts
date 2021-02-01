---
title: VsVim 配置快捷键
siteurl: vsvim-配置快捷键
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-VsVim 配置快捷键'
date: 2020-12-14T21:26:27.000Z
date updated: '2020-12-26T11:38:59+08:00'

---

## 1. 配置快捷键

1.四种模式：normal,insert,visual,command
2.映射
map:imap,nmap,vmap,cmap
noremap:inoremap,nnoremap,vnoremap,cnoremap
3.Visual Studio 快捷键
0.VsVim配置
Optins->VsVim->
Defaults:Use Editor Command Margin->False (否则，有命令框)
Keyboard:set your Keyboard ->include All Scopes
1.VsVimrc路径
home下，_vsvimrc
2.Vs命令查看方式：
英语环境
Tools->Options->Environment->Keyboard
View->OtherWindow->Command Window
3.在vim中调用快捷命令
:vsc VAssistX.OpenFileInSolutionDialog<cr>
2.vim的常用命令
1.normal模式下
1.移动
.基本移动
hjkl
j:下
k:上
h:左
l:右
.单词移动
w:word  "跳转到单词的头"
W:WORD
e:word's end   "跳转到单词的结尾"
E:WORD's end
ge:word's end "跳转到上一个单词的结尾"
gE:WORD's end
b:back "跳转到单词的头"
B:back "跳转到WORD的头"
.行移动
_： 跳转到行的第一个字符
0:  跳转到行首
$:  跳转到行的最后一个字符
.缩进行

> > :向右缩进

<< :向左缩进
.段落移动：
{: 向上移动一个段落
}: 向下移动一个段落
[[:
]]:
{[:
}]:
.屏幕移动
gg: 跳转到文件首部
G:  跳转到文件结尾
12gg:   跳转到第12行
zz: 当前光标移动到屏幕最中央
zt: 当前光标移动到屏幕最上方（top）
zb: 当前光标移动到屏幕最下方（bottom）
c-f:    下翻半页
c-u:    上翻半页
2.进入插入模式
i:  插入到光标前
I:  插入到行的最前面
a:  插入到光标后
A:  插入到行的最后面
s:删除光标下的字符，并进入插入模式
S:删除当前行所有字符，并进入插入模式
ce/cw: 删除当前光标到word末尾处的字符，进入插入模式
C:删除当前光标后的所有字符，并进入插入模式

        o:插入到下一行
        O:插入到上一行
    3.撤销和reDo
        u:  撤销
        c-r: redo
    4.替换
        r:  用输入替换光标下的字符
        R:进入替换模式
    5.查找：
        .单行
        f{char}:    从当前光标向后查找char。
        F{char}:    从当前光标向前查找char
        t{char}:    从当前光标向后查找char,光标落到char之前
        T{char}:    从当前光标向前查找char,光标落到char之后
            ; :下一个char
            , :上一个char
        .整个文件中查找
        / : 从当前行向下查找
        ? : 从当前行向上查找
        * : 从当前向下，查找当前光标下的word 
        # : 从当前向上，查找当前光标下的word 
        n : 下一个查找结果
        N : 上一个查找结果
    6.删除
        x:剪切当前光标下的字符
        X:剪切当前光标前的一个字符

        d{motion}:删除
        dl: 删除当前光标下的一个字符
        de/dw: 删除当前光标下到word end的字符
        db: 删除当前光标下，到word head的字符
        D:  删除当前光标下到行尾的字符
    7.复制
        y{motion}:复制
        yl: 复制当前光标下的一个字符
        yy/Y：复制当前行
        ye/yw: 复制当前光标到word end的字符
        yb:复制当前光标前一字符到word head的字符
    8.粘贴
        p:粘贴到当前光标后
        P:粘贴到当前光标前

    9.点范式（.）
        . :重复上次的修改(移动，删除，增加)
    10.组合命令
        yyp:复制当前行到下一行
        ddp:和下一行交换（删除当前行，复制到下一行）
        xp:交换两个chr的位置

        yi(:复制括号()中的内容 yi{ yi[ yi" yi'
        di(:删除括号()中的内容 di{ di[ di" di'
        %:跳转匹配的{}、()、[]
    11.修改文本
       J:将光标所在的下一行和本行连接,并且中间隔一个空格
       gJ:将光标所在的下一行和本行连接(下一行的空格也连接)

2.insert模式下
c-o:暂时进入normal模式。一次命令后，回到插入模式。

3.visual模式(visual(可视行，可视块))）,select(选择))
normal模式进入可视模式：
v:字符可视模式
V:行visual
c-v:块visual
退出可视模式：
行/块visual  v-> visual v-> normal
字符可视模式  v ->normal
<esc> or <c-[>
块模式下插入：
c-V：进入块模式，并选择好区域
I    在所选区域前，进入块插入模式(插入的内容会在所有列都插入)
A    在所选区域后，进入块插入模式(插入的内容会在所有列都插入)
c    删除所选区域内容，并进入块插入模式(插入的内容会在所有列都插入)
C    删除所选区域以及所选区域后的所有内容，并且进入块插入模式(插入的内容会在所有列都插入)
<esc> 退出到normal模式

    c-g:切换可视模式和选择模式

    可用命令：
        移动：hjkl
        查找：f{char} ; , n/N
        删除：c C d x s
    其他命令：
        o: 切换选中区域的首和尾
        U: 选中的字符全部大写 
        u: 选中的字符全部小写
        ~：选中的字符切换大小写

    组合命令：
    ve/vw/viw:选中一个单词
    vu:光标下的字符小写
    vU:光标下的字符大写
    veu/U:选中一个单词小写/大写

4.command模式
在normal模式或者visual模式下输入冒号:,进入命令模式
normal模式下的命令，可以在命令模式下运行
command下也可以运行ex命令
:com[mand]  列出所有用户自定义的命令
基本命令：
:w[a]  [全部]写入（保存）
:q[a]  [全部]退出\
:c[a]  [全部]关闭buffer
:x[a]  [全部]和:wq类型，当文件有修改时候，会写入

    :[range]copy/co/t {address}  将范围内的行复制到目标地址处  {address}使用符号
    :[range]move {address}    将范围内的行移动到目标地址处

    :normal 命令，在命令行中执行normal模式下的命令
    如果是visual模式，会呈现
    :'<,'>normal    会在每一行都执行命令

5.命令范围
.command模式下
vim中，在单个文件中，
1.以行号为地址
2.此外，还有一些特殊符号表示地址，例如：
%   整个文件（:1,$ 的简写形式）
            .   光标所在位置。当前位置
            1   文件第一行
             $   文件最后一行
0   虚拟行，位于文件第一行上方
'm  包含标记m的行
'<  高亮选区的起始行
'>  高亮选区的结束行

        范围的表示用逗号隔开，例如 add1,add2
        :12,15d 表示删除12行到15行内容
        :[range]d[elete] [x] {count}    "x到代表寄存器x里"
    .normal模式下
        normal中的一些命令，可以指定运行次数，例如：
            2dd 运行两次dd->删除两行
            2yy 运行两个yy->复制两行

3.mark和跳转
mx:添加本文件"书签"x
mX:添加全局"书签"X
`x:跳转到书签x所在的行和列
'x:跳转到书签x所在行
``:跳转到本文件上次离开的位置
gi:跳转到上一次插入的位置
gv:跳转到上一次进入visual的位置
4.VsVim的配置
1.重要配置
set backspace=indent,eol,start "退格键设置"
set clipboard=unnamed   "默认的剪切寄存器"
set ignorecase  "忽略大小写"
2.推荐配置
1.注释
:vnoremap ci :s/^///<cr>
:vnoremap cu :s////<cr>
:nnoremap ci :s/^///<cr>
:nnoremap cu :s////<cr>
2.相关配置
.单个文件中：
:inoremap jj <esc>  "退出插入模式"
:noremap gd <c-]>zz "跳转到定义"
:nnoremap gq ==     "代码格式format"
:vnoremap gq ==
:nnoremap <space> za "折叠"
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
        :nnoremap <c-o> :vsc View.NavigateBackward<CR>  
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

        "end

3.配置文件
source filefullPath     "读取filefullPath的配置内容，并运行"
5.寄存器介绍和使用
vim提供了几十组寄存器，用于保存文本
为命令添加 "{register}  表示指定要使用的寄存器。若不指定，将缺省用unnamed无名寄存器
"x      表示引用了寄存器x

""      unnamed 无名寄存器,如果没有指定要使用的寄存器，vim会缺省使用此寄存器
"_      黑洞寄存器(实际上并没有存储，如果"_d{motion}会执行真正的删除)
"0      复制专用寄存器  (复制的时候，不仅会把内容拷贝到无名寄存器中，还会拷贝到此寄存器)
"a-"z   a-z寄存器(26个英文字母都是寄存器的名字)
"+      系统剪贴板寄存器 (使用"+p来粘贴系统剪贴板的内容)(X11剪切板，用剪切、复制与粘贴命令操作)
"*      选择专用寄存器 (X11主剪切板，用鼠标中断操作)
"=      表达式寄存器.在插入模式下c-r= ,会进入命令行模式，输入表达式回车之后的结果将插入文本中
"%      存储了当前文件名
"#      存储了轮换文件名
".      存储了上次插入的文本
":      存储了上次执行的Ex命令
"/      存储了上次查找的模式

使用示例：
"fyy    将复制的内容存储到f寄存器
"fp     将f寄存器中的内容粘贴出来

:reg[ister] "{register}     查看{register}中的内容
6.宏
把命令序列录制成宏
在normal,q键开始/停止录制宏。
开始录制：
q{register} 表示指定将宏录制到哪个寄存器中
停止录制：
再次按q,停止录制

@{register} 表示执行寄存器中的命令
@@  重复最近调用过的宏
[count]@{register}  执行count次宏       串行方式
:reg {register} 查看寄存器中的内容

追加：
qa  录制按键操作，并覆盖a寄存器中原有内容
qA  录制按键操作，并追加到a寄存器中
在插入模式中，插入寄存器中的内容：
c-r{register}   将寄存器中的内容插入到文本中
.多次执行宏：
串行和并行。在处理错误的方式中，串行遇到错误，就返回。
1.串行
[count]@{register}
2.并行
:'<,'>normal @{register}
.迭代求值的方式给列编号
使用了表达式寄存器(=)，以便插入的时候，可以进行运算
(不要试图控制每次执行多少遍宏（vim 函数）)
let i=0
qa 0f[ls<c-r>=i<esc>j
:let i+=1
q
.编辑宏
宏中使用的是键盘编码，
如果使用<c-r>{register}的方式获取的寄存器内容不一样。
不过，可以使用将寄存器中的内容粘贴出来的方式。
1.输出到文本中编辑
"{register}p        会粘贴到光标后,所以，用:put 输出到下一行
:put {register}
将寄存器内容输入到文本中
编辑完成之后，
"{register}dd       注意，Windows下，可能会将末尾的^M也粘贴进去。可以使用选中->删除到寄存器中
将文本内容删除并读取到寄存器中
2.使用函数
:let @{register}=......
7.替换
:[range]s/{original}/{new}/[g]  g表示全部替换，否则只替换第一个遇到的
1
8.小技巧
1.
ctrl+h 删除前一个字符，相当于退格键
ctrl+w 删除前一个单词
ctrl+u 删除至行首
2.
num<C-a> :第一个遇到的数字加num
num<C-x> :第一个遇到的数字减num
tips:
:help [   index帮助文档中，所有命令

快捷键映射可以自己修改吗8月前回复
点赞
yanchezuo
延澈左回复:可以。例如 :nnoremap zm :vsc VAssistX.ListMethodsInCurrentFile<cr> "函数列表"。表示把zm映射为显示函数列表

vi/vim多行注释、取消多行注释、多行复制、多行删除

ForeseeMark 2017-05-04 17:22:16  3016  收藏
分类专栏： vim技巧 文章标签： vim 注释 多行操作 vim技巧
版权
多行注释
进入命令行模式，按ctrl + v进入 visual block模式（可视快模式），然后按j, 或者k选中多行，把需要注释的行标记起来

按大写字母i，再插入注释符，例如//

按esc键就会全部注释了

取消多行注释：
进入命令行模式，按ctrl + v进入 visual block模式（可视快模式），按小写字母L横向选中列的个数，例如 // 需要选中2列

按字母j，或者k选中注释符号

按d键或x键就可全部取消注释

多行复制
如果知道需要复制几行
将光标移至需要复制的内容的第一行，计算需要复制多少行，比如需要复制5行

按下5yy，将光标移至需要复制的地方，按下p粘贴

如果不知道需要复制几行
有时候不想费劲看多少行或复制大量行时，可以使用标签来替代

1.  光标移到起始行，输入ma
2.  光标移到结束行，输入mb
3.  光标移到粘贴行，输入mc
4.  然后 :’a,’b co ‘c 把 co 改成 m 就成剪切了

多行删除
跟多行复制的情况差不多，第一种方法里5yy改成5dd就行，第二种方法里，或者用:5, 9 de。
