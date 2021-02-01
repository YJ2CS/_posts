---
title: C语言中time()函数简介
siteurl: c语言中的time-srand函数
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - C语言
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-2230'
date: 2020-11-10T22:16:00.000Z
date updated: '2020-12-23T00:21:29+08:00'

---

所在头文件：time.h

函数原型：time_t time(time_t * timer)

功能: 获取当前的系统时间，

返回的结果是一个time_t类型，其实就是一个大整数，其值表示从CUT（Coordinated Universal Time世界标准时间、国际协调时间）

时间1970年1月1日00:00:00（称为UNIX系统的Epoch时间）到当前时刻的秒数。

然后调用localtime将time_t所表示的CUT时间转换为本地时间（我们是+8区，比UTC多8个小时）并转成struct tm类型，

该类型的各数据成员分别表示年月日时分秒。

补充说明：time函数的原型也可以理解为 `long time`(`long *tloc`)，即返回一个long型整数。因为在time.h这个头文件中time_t实际上就是：

```cpp
#ifndef  _TIME_T_DEFINED
#typedef  long time_t; /* time value */
#define  _TIME_T_DEFINED /* avoid multiple defines of time_t */
#endif
```

即long

应用举例

程序例1:

time函数获得日历时间。

日历时间，是用“从一个标准时间点到此时的时间经过的秒数”来表示的时间。

这个标准时间点对不同的编译器来说会有所不同，但对一个编译系统来说，这个标准时间点是不变的，

该编译系统中的时间对应的日历时间都通过该标准时间点来衡量，所以可以说日历时间是“相对时间”，

但是无论你在哪一个时区，在同一时刻对同一个标准时间点来说，日历时间都是一样的。

```cpp
#include <time.h>
#include <stdio.h>
#include <dos.h>
int main(void)
{
time_t t; t = time(NULL);
printf("The number of seconds since January 1, 1970 is %ld",t);
return 0;
}
```

程序例2：

```c
//time函数也常用于随机数的生成，用日历时间作为种子。
#include <stdio.h>
#include <time.h>
#include<stdlib.h>
int main(void)
{
int i;
srand((unsigned) time(NULL));
printf("ten random numbers from 0 to 99\n\n");
for(i=0;i<10;i++)
{
printf("%d\n",rand()%100);
}
return 0;
}
```

程序例3：
用time()函数结合其他函数（如：localtime、gmtime、asctime、ctime）可以获得当前系统时间或是标准时间。

```c
#include <stdio.h>
#include <stddef.h>
#include <time.h>
int main(void)
{
time_t timer;//time_t就是long int 类型
struct tm *tblock;
timer = time(NULL);//这一句也可以改成time(&timer);
tblock = localtime(&timer);
printf("Local time is: %s\n",asctime(tblock));
return 0;
}

```

## C语言中srand()函数简介

所在头文件：stdlib.h

srand函数是随机数发生器的初始化函数。

函数原型：void srand(unsigned int seed);srand和rand()配合使用产生伪随机数序列。

函数用法

rand函数在产生随机数前，需要系统提供的生成伪随机数序列的种子，rand根据这个种子的值产生一系列随机数。

如果系统提供的种子没有变化，每次调用rand函数生成的伪随机数序列都是一样的。

srand(unsigned seed)通过参数seed改变系统提供的种子值，从而可以使得每次调用rand函数生成的伪随机数序列不同，从而实现真正意义上的“随机”。

通常可以利用系统时间来改变系统的种子值，即srand(time(NULL))，可以为rand函数提供不同的种子值，进而产生不同的随机数序列

使用举例

随机输出十个0-100之间的整数

例1（C语言）

```c
#include<stdlib.h>/*用到了srand函数，所以要有这个头文件*/
#include<stdio.h>
#define MAX 10
 
int main(void)
{
int number[MAX] = {0};
int i;
unsigned int seed;
scanf("%d",&seed);/*手动输入种子*/
srand(seed);
for(i = 0; i < MAX; i++)
{
number[i] = (rand() % 100);/*产生100以内的随机整数*/
printf("%d\n",number[i]);
}
printf("\n");
return 0;
}
```

例2（C语言）

```c
#include<stdlib.h>
#include<stdio.h>
#include<time.h> /*用到了time函数，所以要有这个头文件*/
#define MAX 10
 
int main(void)
{
int number[MAX] = {0};
int i;
srand((unsigned)time(NULL));/*播种子*/
for(i = 0; i < MAX; i++)
{
number[i] = (rand() % 100);/*产生100以内的随机整数*/
printf("%d\n",number[i]);
}
printf("\n");
return 0;
}
```
