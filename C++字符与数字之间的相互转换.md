---
title: C++字符与数字之间的相互转换
date: 2020-02-03 15:08:40
tags: 字符转数字
categories: C++
---
# 字符串转数字
|函数    |功能|
|:------|:--------|
atof	|将字符串转换成浮点型数
atoi	|将字符串转换成整型数
atol	|将字符串转换成长整型数
strtod	|将字符串转换成浮点数
strtol	|将字符串按指定进制转换成长整型数
strtoul	|将字符串按指定进制转换成无符号长整型数
toascii	|将字符换成合法的ASCII码
sscanf  |将字符串以指定类型赋值给变量

<!-- more -->
## 例子
```c++
#include <iostream>
#include <cstdlib>
#include <cstdio>
#include <sstream>
using namespace std;
int main()
{
    char *endptr;//当字符串中含有非法字符时赋给endptr
    cout << atof("3.14") << endl;

    cout << atoi("10") << endl;

    cout << atol("21") << endl;

    cout << strtod("5.12",&endptr) << endl;

    cout << strtol("48",&endptr,10) << endl;

    cout << strtoul("81",&endptr,10) << endl;

    cout << toascii('A') << endl;

    char str[20]="56";
    int num;
    sscanf(str, "%d", &num);
    cout << num << endl;

    return 0;
}
```
## 输出结果
```
3.14
10
21
5.12
48
81
65
56
```
---
# 数字转字符串
|函数    |功能|
|:------|:--------|
sprintf |将指定类型的数字转换成字符串
itoa    |将十进制数字以指定进制转换成字符串
to_string|将数字转换成字符串

## 例子
```c++
#include <iostream>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <cstring>
using namespace std;
int main()
{
    char str[20];
    sprintf(str, "%d", 123);
    cout << str << endl;

    itoa(456, str, 10);//非标准库函数，只能在Windows下使用
    cout << str << endl;

    string s = to_string(789);//需支持C++11标准
    cout << s << endl;
    return 0;
}
```

## 输出结果
```
123
456
789
```
---
# 以流的形式完成字符串和数字之间的转换
## 例子
```c++
#include<sstream>
#include<iostream>
using namespace std;
int main()
{
    stringstream ss;
    int num1 = 128;
    ss << num1;
    int num2;
    ss >> num2;
    cout << num2 << endl;

    ss.clear();//将流清空，避免影响后面流的使用

    string str("512");
    //char str[20]="512";
    int num3;
    ss << str;
    ss >> num3;
    cout << num3 << endl;
}
```
## 输出结果
```
128
512
```