---
title: 时间类Time的编写
date: 2020-05-07 16:15:56
tags:
	- 操作符重载
categories: C++
---

## 问题描述

编写一个程序，定义一个时间类Time，包含三个属性： hour, minute 和 second

### 要求通过运算符重载实现如下功能:

- 时间输入输出(>>、<<)；

- 时间增加减少若干(+=、-=)，例：Time& operator+=(const Time&);Time& operator-=(const Time&)；

- 时间前、后自增加/减少1秒(++、--)，前自增例：Time& operator++(); 后自增例：Time operator++(int)；

<!-- more -->

## 输入形式

- 输入固定为两个Time实例(time1，time2),每个实例占一行；
- Time实例输入格式为：hour minute second。

## 输出形式

- Time实例输出格式为：hour:minute:second；

- 每个输出实例占一行。

### 依次输出以下表达式的值

- time1 += (time2++)
- time1 -= time2
- ++time2
- time2 += (time1--)
- --time1
- time2 -= time1

## 样例输入

21 10 35
10 15 25

## 样例输出

07:26:00
21:10:34
10:15:27
07:26:01
21:10:32
10:15:29

## 代码

``` cpp
#include <iostream>
#include <iomanip>
using namespace std;
class Time
{
private:
	int hour;
	int minute;
	int second;

public:
	Time(int h = 0, int m = 0, int s = 0) : hour(h), minute(m), second(s){};
	Time(const Time &other) : hour(other.hour), minute(other.minute), second(other.second){};
	friend istream &operator>>(istream &input, Time &other)
	{
		input >> other.hour >> other.minute >> other.second;
		return input;
	}
	friend ostream &operator<<(ostream &output, const Time &other)
	{
		output << setw(2) << setfill('0') << other.hour << ":" << setw(2) << setfill('0') << other.minute << ":" << setw(2) << setfill('0') << other.second << endl;
		return output;
	}
	const Time operator++(int)
	{
		Time tmp(*this);
		int x = hour * 3600 + minute * 60 + second;
		x++;
		hour = x / 3600;
		x %= 3600;
		minute = x / 60;
		second = x % 60;
		return tmp;
	}
	const Time operator--(int)
	{
		Time tmp(*this);
		int x = hour * 3600 + minute * 60 + second;
		x--;
		hour = x / 3600;
		x %= 3600;
		minute = x / 60;
		second = x % 60;
		return tmp;
	}
	Time operator+=(const Time &other)
	{
		second += other.second;
		minute += other.minute;
		hour += other.hour;
		if (second >= 60)
		{
			minute += second / 60;
			second %= 60;
		}
		if (minute >= 60)
		{
			hour += minute / 60;
			minute %= 60;
		}
		if (hour >= 24)
		{
			hour %= 24;
		}
		return *this;
	}
	Time operator-=(const Time &other)
	{
		if(second<other.second)
		{
			second += 60;
			second -= other.second;
			minute--;
		}
		else
		{
			second -= other.second;
		}
		if(minute<other.minute)
		{
			minute += 60;
			minute -= other.minute;
			hour--;
		}
		else
		{
			minute -= other.minute;
		}
		if(hour<other.hour)
		{
			hour += 24;
			hour -= other.hour;
		}
		else
		{
			hour -= other.hour;
		}		
		
		return *this;
	}
	Time& operator++()
	{
		int x = hour * 3600 + minute * 60 + second;
		x++;
		hour = x / 3600;
		x %= 3600;
		minute = x / 60;
		second = x % 60;
		return *this;
	}
	Time& operator--()
	{
		int x = hour * 3600 + minute * 60 + second;
		x--;
		hour = x / 3600;
		x %= 3600;
		minute = x / 60;
		second = x % 60;
		return *this;
	}
};

int main()
{
	Time t1, t2;
	cin >> t1;
	cin >> t2;
	cout << (t1 += (t2++));
	cout << (t1 -= t2);
	cout << ++t2;
	cout << (t2 += (t1--));
	cout << --t1;
	cout << (t2-=t1);
}
```

