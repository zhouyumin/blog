---
title: Java进制转换方法整理
date: 2020-03-03 14:22:48
tags:
    - 进制转换
    - 字符转数字
categories: Java
---
## 一、利用Integer将十进制的数进行进制转换

### 方法
|十进制转换其他进制|使用方法| 返回值|
|-------------|---------------------|----------------|
|10进制转2进制  |Integer.toBinaryString(n)|一个二进制字符串|
|10进制转8进制	|Integer.toOctalString(n) |一个八进制字符串|
|10进制转16进制	|Integer.toHexString(n)   |一个十六进制字符串|
|10进制转 R 进制|Integer.toString(100, 16)|一个R进制字符串|

<!-- more -->
### 例子
``` java
public class Main {
	public static void main(String[] args) {
		int n = 13;
		Integer.toHexString(n);
		System.out.println(n + "的二进制是:" + Integer.toBinaryString(n));
		System.out.println(n + "的八进制是:" + Integer.toOctalString(n));
		System.out.println(n + "的十六进制是:" + Integer.toHexString(n));
		System.out.println(n + "的三进制是:" + Integer.toString(n, 3));
	}
}
```
### 结果
```
13的二进制是:1101
13的八进制是:15
13的十六进制是:d
13的三进制是:111
```
---
## 二 利用Integer.valueOf将字符串解析成十进制
### 用法
Integer.valueOf(s, radix) 将s以radix进制的形式转换成十进制数，返回值为Integer

### 例子
``` java
public class Main {
    public static void main(String[] args) {
        String str = new String("1001011");
        System.out.println("二进制数" + str + "的十进制数为：" + Integer.valueOf(str, 2));
        str = "123";
        System.out.println("八进制数" + str + "的十进制数为：" + Integer.valueOf(str, 8));
        str = "ffee23";
        System.out.println("十六进制数" + str + "的十进制数为：" + Integer.valueOf(str, 16));
    }
}
```
### 结果
```
二进制数1001011的十进制数为：75
八进制数123的十进制数为：83
十六进制数ffee23的十进制数为：16772643
```
---
## 三、利用Integer.parseInt将字符串解析成十进制

### 用法
Integer.parseInt(s, radix) 将s以radix进制的形式转换成十进制数，返回值为int型，用法和Integer.valueOf一样，就返回值类型不同而已

### 例子
``` java
public class Main {
    public static void main(String[] args) {
        String str = new String("1001011");
        System.out.println("二进制数" + str + "的十进制数为：" + Integer.parseInt(str, 2));
        str = "123";
        System.out.println("八进制数" + str + "的十进制数为：" + Integer.parseInt(str, 8));
        str = "ffee23";
        System.out.println("十六进制数" + str + "的十进制数为：" + Integer.parseInt(str, 16));
    }
}
```

### 结果
```
二进制数1001011的十进制数为：75
八进制数123的十进制数为：83
十六进制数ffee23的十进制数为：16772643
```
---
## 四、利用 BigInteger 实现进制转换

### 用法
BigInteger(string, base).toString(to) 使用BigInteger将string以base进制的形式转换成to进制

BigInteger(string, base) 将string以base进制的形式解析为BigInteger类型

BigInteger.toString(to) 将BigInteger转换成to进制的String类型

### 例子
``` java
import java.math.BigInteger;
public class Main {
    public static void main(String[] args) {
        String string = new String("abcdef123");
        int base = 16;
        int to = 8;
        System.out.println(base+"进制数 "+string+" 转换成"+to+"进制数为："+new BigInteger(string,base).toString(to));
    }
}
```
### 结果
```
16进制数 abcdef123 转换成8进制数为：527467570443
```
>https://blog.csdn.net/szwangdf/article/details/2601941
>https://blog.csdn.net/qq_40358862/article/details/88934619
>https://blog.csdn.net/m0_37961948/article/details/80438113