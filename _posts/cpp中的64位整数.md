---
title: C++中的64位整数
siteurl: cpp中的64位整数
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - Cpp
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-7cb9'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-23T00:22:04+08:00'

---

在做ACM题时，经常都会遇到一些比较大的整数。而常用的内置整数类型常常显得太小了：

其中long 和 int 范围是

```text
[-2^31,2^31)，
```

即-2147483648~2147483647。

而unsigned范围是

```text
[0,2^32)，
```

即0~4294967295。

也就是说，常规的32位整数只能够处理40亿以下的数。

那遇到比40亿要大的数怎么办呢？这时就要用到C++的64位扩展了。不同的编译器对64位整数的扩展有所不同。基于ACM的需要，下面仅介绍VC6.0与g++编译器的扩展。

　　VC的64位整数分别叫做__int64与unsigned __int64，其范围分别是

```text
[-2^63, 2^63)与[0,2^64)，
即-9223372036854775808~9223372036854775807与0~18446744073709551615

```

(约1800亿亿)。对64位整数的运算与32位整数基本相同，都支持四则运算与位运算等。当进行64位与32位的混合运算时，32位整数会被隐式转换成64位整数。

但是，VC的输入输出与__int64的兼容就不是很好了，如果你写下这样一段代码：

    1 __int64 a;
    2 cin >> a;
    3 cout << a;

那么，在第2行会收到“error C2679: binary '>>' : no operator defined which takes a right-hand operand of type '__int64' (or there is no acceptable conversion)”的错误；在第3行会收到“error C2593: 'operator <<' is ambiguous”的错误。

那是不是就不能进行输入输出呢？当然不是，你可以使用C的写法：

scanf("%I64d",&a);

printf("%I64d",a);

就可以正确输入输出了。

当使用unsigned __int64时，把"I64d"改为"I64u"就可以了。

　　OJ通常使用g++编译器。其64位扩展方式与VC有所不同，它们分别叫做long long 与 unsigned long long。处理规模与除输入输出外的使用方法同上。对于输入输出，它的扩展比VC好。既可以使用

```C
long long a;
cin>>a;
cout<<a;也可以使用
scanf("%lld",&a);
printf("%lld",a);
```

使用无符号数时，将"%lld"改成"%llu"即可。

　　最后补充一点：作为一个特例，如果你使用的是Dev-C++的g++编译器，它使用的是"%I64d"而非"%lld"。
