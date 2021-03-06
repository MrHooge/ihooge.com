---
title: C语言学习笔记
date: 2016-01-07 11:20:58
tags: 后端语言 C语言
---

### Day1：数据类型、变量、内存四区、指针基础
数据类型提供了，申请内存单元的大小和访问规则。
    基本类型： char、short、int、long int、float、double、long double。
    构造类型：数组、结构体、共用体、枚举、指针。
    补码：计算机的基石；
    补码的运算规则：正数：本身；负数：取反加1； 补码的运算规则是可逆的。
    无符号：0~255 有符号：-128~127；
    类型转换：
        小数据赋给大变量：
            不会造成数据的丢失，系统为了保证数据的完整性，还提供了符号扩充行为。
        大数据赋给小变量
            会发生 truncate(截断行为)，有可能会造成数据丢失。
        隐式转化整型提升
        在 32 位机中，所有位为低于 32 的整型数据，在运算过程中先要转化为 32 位的整型数据，然后才参与运算。
    进程空间：
        程序到进程
            程序：源文件经编译器，编译后的可执行文件。程序是一个静态的概念。程序中包含 3 个区域分别是：text initial data uninial data.
            进程：被操作系统加载至运行结束的过程。进程是一个动态的概念。进程包含 5 个区域分别是：text initial data uninitial dataheap stack

函数调用模型：
        变量三要素：名称、大小、作用域。
<!--more-->
### Day2：C语言中的字符串、一维数组、二维数组
被引号引起的一串字符，就是字符串。
常用字符数组存放字符串：char buf[]=”abcdefg”;
读越界，常常带来写越界，读越界，往往并不会造成内存崩溃。而写越界，则会造成数据破环，严重会造成内存崩溃。
基本的字符串处理函数：strlen strcmp strcat strcpyatoi函数功能：将字符串转换成整型数；atoi()会扫描参数 nptr 字符串，跳过前面的空格字符，直到遇上数字或正负号才开始做转换，而再遇到非数字或字符串时（’\0’）才结束转化，并将结果返回（返回转换后的整型数）
strstr函数功能：strstr() 函数搜索一个字符串在另一个字符串中的第一次出现。找到所搜索的字符串，则该函数返回第一次匹配的字符串的地址；如果未找到所搜索的字符串，则返回 NULL。

字符串函数的自实现
```C
*myStrlen*
> int myStrlen(const char * str)
    {
        int n=0;
        while(*str++)
        n++;
        return n;
    }

*myStrcat*
> char* myStrcat(char *dest,const char*src)
{
    char *ret = dest;
    dest += strlen(dest);
    while(*dest++ = *src++);
    return ret;
}

*myStrcpy*
> char* myStrcpy(char *dest,const char*src)
{
    char *ret = dest;
    while(*dest++ = *src++);
    return ret;
}

*myStrcmp*
> int myStrcmp(const char *str1,constchar*str2)
{
    while(*str1 !=’\0′ &&*str2 != ‘\0′ && *str1==*str2)
    {
        str1++;
        str2++;
    }
    return (*str1== *str2 ? 0 : (*str1 > *str2 ? 1 : -1));
}

*myStrstr*
> char * myStrstr(const char *s1,constchar *s2)
{
    if(*s2)
    {
        while(*s1)
        {
        for(int n=0;*(s1+n)==*(s2+n) ; n++)
        {
            if(*(s2+n+1)==’\0’)
            return s1;
        }
        s1++;
        }
    return NULL;
    }
    else
    return NULL;
}  

更简洁版：

int n = strlen(s2);
for(;(s1 = strchr(s1,*s2))!=NULL;s1++)
{
    if(strncmp(s1,s2,n) == 0)
    return s1;
}
return NULL;
```


#### 一维数组：
    数据类型是一种构造类型，内存是一段连续的存储区域。
    数据类型，决定了连续内存的访问方式。包括，起始地址，步长，寻址范围。
    数组名是数组的唯一标识符，数组的每一个元素都是没有名字的。
    数据名有两重含义：
    作为数组名时:他是一种构造数据类型。
    作为访问成员时:作为数组名，访问成员时，它是首元素的地址。
    作参数传递:当一维数组作参数的时候，要想到，他是一个构造类型，要想到的是，类型的起始地址，步长，寻址范围，有没有传递过去。
#### 二维数组
    二维数组的本质也是一个一维数组，只是数组成员，由基本数据类型变成了，构造数据类型（一维数组）。
    二维空间，一维存储:所以既可以通过二重循环访问，也可以通过一重循环访问。
    数组指针声明
        int (*pName)[N];
    语法解析：“()”的优先级比“[]”高， “*”号和 pName 构成一个指针的定义,指针的类型为 int [N]别名typedef int (*type)[N];
    应用
    传递二维数组的时候，作参数指针的本质：&取地址运算符：取得是的变量所属内存低位字节的地址。内存的地址，称为指针。
    指针变量：指针变量的本质是一个变量，32 位机的平台下，大小为 4 个字节。
    二级指针：所有类型的二级指针，步长都是 4 个字节。但他所指向的一级指针的步长，却由其类型决定。
    常见用法：
        传一级指针，改变指针指向的内容。
        传二级指针，改变一级指针的指向。
    小结：
    以能过一级指针，修改 0 级指针（变量）的内容。
    可以通过二级指针，修改一级指针的指向。
    可以通过三级指针，修改二级指针的指向。
                ·····
    可以通过 n 级指针，修改 n-1 级指针的指向。

## Day3：指针数组和数组指针
    指针数组（字符指针数组)：指针数组的本质是数组，数组中每一个成员是一个指针。
    二级指针与指针数组名等价的原因
        char**p 是二级指针，char* array[N]; array= &array[0]; array[0] 本身是 char*型
    访问方式：指针方式
        指针访问方式，比数组访问方式少了一个维度的概念，所以，通过指针方式访问，要么知道维度.
    指针的输入与输出
        输入，特指主调函数申请空间，输出指被调函数申请空间。
        二维空间，并不一定就是二维数组，具有数组的访问形式。但己经远远不是数组的定义了。
    二维指针跟二维数组
        两者没有关系，一个指针数组，而另一个是数组指针。
    数组指针与二维数组名
    论等价是有条件的
        int a[3][4]; int (*p)[4] 维数 3
    给访问带来的便利
    可以任意的一维空间，当作二维的方式来访问。

以上是我这几天的复习笔记，希望对一些初级朋友有帮助。不足之处还望批评指正！！
初级菜鸟献丑啦！！嘿嘿·······