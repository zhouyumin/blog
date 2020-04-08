---
title: 编译Qt的opencv库
date: 2020-02-18 11:30:29
tags:
    - opencv
    - qt
categories: opencv
---

## 注意事项
opencv不要用高版本的，否则编译不出来（这是个玄学问题），应该是qt的mingw53_32编译器版本低吧，这里建议用opencv3，官方教程用的是opencv3.2.0
但是opencv3.2版本不自带dnn模块还得另外安装，所以这里推荐opencv3.3以上的，我用的是opencv3.4.3

<!-- more -->


## 官方教程
https://wiki.qt.io/How_to_setup_Qt_and_openCV_on_Windows

## youtube视频教程
https://www.youtube.com/watch?v=ZOSu-2Oju-A

## 解决编译中所遇到的bug

不勾选WITH_MSMF

勾选ENABLE_CXX11

编辑opencv3.4.3\opencv\sources\modules\videoio\src\cap_dshow.cpp文件，在约110行左右`#include "DShow.h"`上面添加
``` cpp
#define NO_DSHOW_STRSAFE
#define STRSAFE_NO_DEPRECATE
```

config后再generate

## 我编译好的库
链接：https://pan.baidu.com/s/18lmzp0ovoplGbr_orHBp-A 

提取码：7yj4

## Tips
该加的环境变量记得加，否则你编译不了或用不了opencv，这个道理懂得吧

**官方有教程这里就不赘述了**