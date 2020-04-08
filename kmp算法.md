---
title: kmp算法
date: 2020-03-14 11:32:41
tags:
     - kmp
categories: 算法
---
## 构造前缀表
前缀表记录的值是以该点为终点的前缀和后缀相等的最大长度

<!-- more -->

先以字符串` substr="ababaaaba"` 为例

|i|截止到 i 点的子串|前缀|后缀|长度 len|
|-|---------------|---|---|--|
|0|a              |无 |无  |0 |
|1|ab             |无 |无  |0 |
|2|aba            |a  |a  |1 |
|3|abab           |ab |ab |2 |
|4|ababa          |aba|aba|3 |
|5|ababaa         |a  |a  |1 |
|6|ababaaa        |a  |a  |1 |
|7|ababaaab       |ab |ab |2 |
|8|ababaaaba      |aba|aba|3 |

我们可以发现当` substr[i]` 等于` substr[len]` 时，i 的长度len就是上一个长度len+1

不相等时len就回溯到上一个第len-1个字符子串的len（这个很难理解，记住就好）

## 代码
``` cpp
#include <iostream>
#include <string>
using namespace std;

//ababaaaba
void getNext(string &substr, int *next)
{
    next[0] = 0;//前缀表
    int i = 1;
    int len = 0;
    while (i < substr.length())
    {
        if (substr[i] == substr[len])
        {
            //如果能匹配的话，i点前缀就是前一点的前缀加一
            len++;
            next[i] = len;
            i++;
        } 
        else
        {
            if (len > 0)
                len = next[len - 1];//不能匹配的话就回溯
            else
            {//到0点前缀就为0了
                next[i] = len;
                i++;
            }
        }
    }
}

int kmp(string &str, string &substr, int *next)
{
    int i = 0;
    int j = 0;
    while (i < str.length())
    {
        if (j == substr.length())
        {
            cout << i - j << endl;
            i++;
            j = next[j - 1];
        }
        if (str[i] == substr[j])
        {
            i++;
            j++;
        }
        else
        {
            if (j - 1 < 0)
            {
                i++;
                j++;
            }
            else
            {
                j = next[j - 1];
            }
        }
    }
}

int main()
{
    string str("abdhvdjajababaaabadfdababaaabag");
    string substr("ababaaaba");
    int *next=new int[substr.length()];
    getNext(substr, next);
    kmp(str, substr, next);
    delete[] next;
    next = NULL;
}
```