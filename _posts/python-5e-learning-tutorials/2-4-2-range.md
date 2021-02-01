---
title: 2-4-2-range
siteurl: 2-4-2-range
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
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-2-4-2-range'
date: 2020-12-20 00:30:00
---



## range
这里其实还涉及一个函数重载的问题，就是range函数有三种 
第一种默认从0开始生成数
range(MaxValue)
指定最大值MaxValue，
一共生成
0,1,2,3,4,....,MaxValue-1

第二种
range(FirstValue,MaxValue)
一共生成
FirstValue,FirstValue+1,FirstValue+2,...,MaxValue-1

第三种
range(FirstValue,MaxValue,step)
一共生成
FirstValue,FirstValue+1*step,FirstValue+2*step,....,
这里注意最后一个数不会超过MaxValue-1

字符串是序列的一种特例
序列类型还包括列表和元组

exec模式（放在中期）联合调用
import 和reload
import和from