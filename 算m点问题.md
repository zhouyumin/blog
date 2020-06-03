---
title: 算m点问题
date: 2020-05-29 14:21:41
tags:
	- 回溯法
categories: 算法
---

## 问题描述

给定k个正整数，用算术运算符+，-，*，/将这个k接起来，使最终的复数恰为m

**算法设计**：对于任意给定的k个整数，给出计算m的算术表达式。若无解，则输出“No solution!”

<!-- more -->



## 分析

在进行回溯的时候对于每一个数都可以选择用或者不用，如果用的话则有加、减、乘、除四种情况

用的时候分别对这四种进行回溯，如果最终有解的话则把当前计算的式子保存到栈中，最后一次性输出

用visit数组标记每个数是否使用过，用flag标记是否有解

## 代码

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <stack>
using namespace std;

int n;             //数的个数
int m;             //目标数
int *nums;         //n个数
bool *visit;       //标记是否使用
bool flag = false; //标记是否有解

stack<string> s;
string str;
bool backtrack(int dept, int value)
{
    if (dept > n - 1)
    {
        if (value == m)
        {
            flag = true;
            return true;
        }
        else
        {
            return false;
        }
    }
    for (int i = 0; i < n; i++)
    {
        if (visit[i] == false)
        {
            stringstream ss;
            visit[i] = true; //使用该数

            if (backtrack(dept + 1, value + nums[i]) && dept != 0)
            {
                ss << value << "+" << nums[i] << "=" << value + nums[i];
                ss >> str;
                s.push(str);
                return true;
            }
            if (backtrack(dept + 1, value - nums[i]) && dept != 0)
            {
                ss << value << "-" << nums[i] << "=" << value - nums[i];
                ss >> str;
                s.push(str);
                return true;
            }
            if (backtrack(dept + 1, value * nums[i]) && dept != 0)
            {
                ss << value << "*" << nums[i] << "=" << value * nums[i];
                ss >> str;
                s.push(str);
                return true;
            }
            if (backtrack(dept + 1, value / nums[i]) && dept != 0)
            {
                ss << value << "/" << nums[i] << "=" << value / nums[i];
                ss >> str;
                s.push(str);
                return true;
            }
            visit[i] = false; //不使用该数
        }
    }
    return false;
}
int main()
{
    cin >> n >> m;
    nums = new int[n];
    visit = new bool[n];
    for (int i = 0; i < n; i++)
    {
        cin >> nums[i];
        visit[i] = false;
    }

    backtrack(0, 0);

    while (!s.empty())
    {
        cout << s.top() << " ";
        s.pop();
    }
    if (flag == false)
    {
        cout << "No solution!" << endl;
    }
    delete[] nums;
    delete[] visit;
}
```

## 输入

```
5 125
7 2 2 12 3
```

## 输出

```
7*12=84 84*3=252 252-2=250 250/2=125
```

