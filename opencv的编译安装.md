---
title: opencv的编译安装
date: 2020-02-17 13:32:19
tags: 
    - opencv
categories:
    - opencv
---
## 下载源码
https://github.com/opencv/opencv/releases

## 下载cmake工具
``` bash
$ sudo apt install cmake  
```
<!-- more -->

## 安装依赖
``` bash
$ sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libtiff4-dev libswscale-dev libjasper-dev
```

## 准备编译
1.解压 opencv源码包

2.进入opencv目录

3.创建编译目录并进入
``` bash
$ mkdir build
$ cd build
```
4.3、执行以下命令
``` bash
$ cmake -D CMAKE_BUILD_TYPE=Release -D OPENCV_GENERATE_PKGCONFIG=YES -D CMAKE_INSTALL_PREFIX=/usr/local/opencv4 ..

```
### 命令说明：

-D OPENCV_GENERATE_PKGCONFIG=YES：OpenCV4以上版本默认不使用pkg-config，该编译选项开启生成opencv4.pc文件，支持pkg-config功能。
-D CMAKE_INSTALL_PREFIX=/usr/local/opencv4：指定安装目录。

## 编译
``` bash
$ make
```

## 安装
``` bash
$ sudo make install
```
## 添加库路径
``` bash
$ sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv4.conf'
```

## 更新系统库
``` bash
$ sudo ldconfig
```

## 添加环境变量
将`/usr/local/opencv4/lib/pkgconfig/`路径加入PKG_CONFIG_PATH：
``` bash
$ sudo vim /etc/profile.d/pkgconfig.sh
```
加入以下语句
``` bash
export PKG_CONFIG_PATH=/usr/local/opencv4/lib/pkgconfig:$PKG_CONFIG_PATH
```
激活
``` bash
$ source /etc/profile
```
查看是否配置成功

``` bash
$ pkg-config --modversion opencv4
```
如果当前终端可以使用，而新开终端没配置成功的话请重启

>参考：https://blog.csdn.net/new_delete_/article/details/84797041