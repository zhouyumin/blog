---
title: labelImage的简单使用
date: 2020-02-07 01:04:32
tags: labelImage
categories: 目标检测
---
# 简介
LabelImg是一个开源的图形图像注释工具，地址：https://github.com/tzutalin/labelImg  
它是用Python编写的，它的图形界面使用PyQt，注释以Pascal VOC格式保存为xml文件，这种格式是ImageNet，此外，它还支持YOLO格式，本文将讲解如何使用LabelImage工具标注图片信息用于训练自己的数据集
<!-- more -->

# 下载安装
你可以去GitHub下载源码并配置好环境直接运行，如果你不会的话或太懒，没事我已经把源代码编译好了，下载解压就能直接用了，废话不多说直接附上下载地址：https://pan.baidu.com/s/1Q9tElvZTcN5A-OGpmienHA  
适用于Win10

# 使用方法
## 1.准备工作
准备两个文件夹JPEGImages和Annotations  
其中JPEGImages里面为你要进行标注的图片,Annotations里面保存你将标注好的信息

## 2.选择标注的数据格式
该程序可以标注VOC格式的也可以标注YOLO格式的，我们打开软件首先就要更改格式，我们要的是YOLO的数据集所以我们点击左侧工具栏中的PascalVoc图标切换为YOLO

![切换格式](/images/pictures/labelImage/1.jpg)

## 3.开始标记
点击Open Dir选择JPEGImages文件夹

点击Change Save Dir选择Annotations文件夹

![工具栏](/images/pictures/labelImage/2.jpg)

按W键或点击Create\nRectBox开始创建矩形框，方法和电脑截图一样，我们把要进行识别训练的区域标记出来就行

**注意：**
标记矩形框一定要紧贴目标，不要过大

选好框后会弹出框让我们选是什么类别，我们根据实际情况选择就行**千万别粗心选错了，否则后果很严重**

整张图片的**所有目标**都标记好了之后按Ctrl+S或点击Save保存 **（记得一定要保存，不然白忙了）** 然后切换下一张继续，**每一张图片标记后都要保存**

如果你没保存就切换图片会有弹框提醒这时我们选中NO这个按钮就能取消切换了

![标记](/images/pictures/labelImage/3.jpg)


# 一些方便的快捷键
|快捷键|功能|
|-----|----|
ESC     |取消正在标记的矩形框
Delete  |删掉选中的已标记好的矩形框
W       |开始创建矩形框
D       |切到下一张图片
A       |切到上一张图片
Ctrl + S|保存
Ctrl + +|放大
Ctrl - -|缩小
↑ → ↓ ← |移动选中的矩形框
Ctrl + U|Open Dir
Ctrl + R|Change Save Dir