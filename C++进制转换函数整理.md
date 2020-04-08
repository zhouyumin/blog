---
title: C++进制转换函数整理
date: 2020-02-03 10:57:53
tags: 进制转换
categories: C++
---
# strtol函数：
## 格式：
```
long int strtol(const char *nptr, char **endptr, int base)
```
## 功能：

它的功能是将一个任意1-36进制数转化为10进制数，返回值是long int型

base是要转化的数的进制，非法字符串会赋给endptr，nptr是要转换的字符串

<!-- more -->

## 列子：

```c++
#include<iostream>
#include<cstdlib>
using namespace std;
int main()
{
    char str[20]="1033180624";
    char *endptr;
    cout<<strtol(str,&endptr,8)<<endl;
    cout<<endptr<<endl;
}
```
## 输出结果：
```
4313
80624
```
在八进制数里8是非法的所以从8开始的后面几个字符为非法字符串赋给endptr，而10331是合法的八进制数，转换成十进制后为4313

---
# itoa函数：

## 格式：
```c++
char *itoa (int value, char *str, int base );
```
## 功能：

它的功能是将一个10进制的数转化为n进制的值、其返回值为char型

value是要转换的十进制数，以base进制转换，转换后的数赋给str

## 注意：

这不是C/C++标准库里的函数，而是Windows平台下的扩展函数，也就是说只能在Windows平台下使用

## 例子：
```c++
#include<cstdlib>
#include<cstdio>
int main()
{
int num = 10;
char str[100];
itoa(num, str, 2);
printf("%s\n", str);
return 0;
}
```
## 输出结果：
```
1010
```
---
# 输出流的定向转换
C++输出流中还有一些定向的转换：

std::bitset（转2进制）  
std::oct（转8进制）   
std::dec （转10进制）  
std::hex（转16进制）

## 例子：
```c++
#include <bitset>
#include <iostream>
using namespace std;
int main()
{
    cout << "36的8进制:" << std::oct << 36 << endl;
    cout << "36的10进制" << std::dec << 36 << endl;
    cout << "36的16进制:" << std::hex << 36 << endl;
    cout << "36的2进制: " << bitset<8>(36) << endl;
    return 0;
}
```
## 输出结果：
```
36的8进制:44
36的10进制36
36的16进制:24
36的2进制: 00100100
```
## 注意：

bitset后面尖括号里的代表输出的位数，例子里为8，所以输出8位二进制数

>参考 https://blog.csdn.net/WangJunchengno2/article/details/78690248