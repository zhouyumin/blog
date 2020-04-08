---
title: pytorch版yolov3的使用
date: 2020-02-14 18:22:24
tags: 
    - pytorch
    - yolov3
categories: 目标检测
---
## 下载yolov3项目
https://github.com/ultralytics/yolov3
<!-- more -->

## 依赖要求
```
numpy

opencv-python

torch >= 1.0.0

matplotlib

pycocotools

tqdm
```
## 数据集制作

请前往我上一篇blog  [lableImage的简单使用](/2020/02/07/lableImage的简单使用)

## 数据准备
将数据集Annotations、JPEGImages复制到YOLOV3工程目录下的data文件下；同时新建两个文件夹，分别命名为ImageSets和labels，最后我们将JPEGImages文件夹复制粘贴一下，并将文件夹重命名为images
## 数据分类
添加脚本makeTxt.py并运行将数据分为训练集和测试集
``` python
import os
import random
trainval_percent = 0.1
train_percent = 0.9
xmlfilepath = 'data/Annotations'
txtsavepath = 'data/ImageSets'
total_xml = os.listdir(xmlfilepath)
num = len(total_xml)
list = range(num)
tv = int(num * trainval_percent)
tr = int(tv * train_percent)
trainval = random.sample(list, tv)
train = random.sample(trainval, tr)
ftrainval = open('data/ImageSets/trainval.txt', 'w')
ftest = open('data/ImageSets/test.txt', 'w')
ftrain = open('data/ImageSets/train.txt', 'w')
fval = open('data/ImageSets/val.txt', 'w')
for i in list:
    name = total_xml[i][:-4] + '\n'
    if i in trainval:
        ftrainval.write(name)
        if i in train:
            ftest.write(name)
        else:
            fval.write(name)
    else:
        ftrain.write(name)
ftrainval.close()
ftrain.close()
fval.close()
ftest.close()
```
## 数据集转换
此项目使用的数据集格式是coco格式的，所以得把Pascal_voc格式的转成coco，转换也很简单添加脚本voc_label.py并运行就能转换格式了
``` python
import xml.etree.ElementTree as ET
import pickle
import os
from os import listdir, getcwd
from os.path import join
sets = ['train', 'test','val']
classes = ["hat","person"]
def convert(size, box):
    dw = 1. / size[0]
    dh = 1. / size[1]
    x = (box[0] + box[1]) / 2.0
    y = (box[2] + box[3]) / 2.0
    w = box[1] - box[0]
    h = box[3] - box[2]
    x = x * dw
    w = w * dw
    y = y * dh
    h = h * dh
    return (x, y, w, h)
def convert_annotation(image_id):
    in_file = open('data/Annotations/%s.xml' % (image_id))
    out_file = open('data/labels/%s.txt' % (image_id), 'w')
    tree = ET.parse(in_file)
    root = tree.getroot()
    size = root.find('size')
    w = int(size.find('width').text)
    h = int(size.find('height').text)
    for obj in root.iter('object'):
        difficult = obj.find('difficult').text
        cls = obj.find('name').text
        if cls not in classes or int(difficult) == 1:
            continue
        cls_id = classes.index(cls)
        xmlbox = obj.find('bndbox')
        b = (float(xmlbox.find('xmin').text), float(xmlbox.find('xmax').text), float(xmlbox.find('ymin').text),
             float(xmlbox.find('ymax').text))
        bb = convert((w, h), b)
        out_file.write(str(cls_id) + " " + " ".join([str(a) for a in bb]) + '\n')
wd = getcwd()
print(wd)
for image_set in sets:
    if not os.path.exists('data/labels/'):
        os.makedirs('data/labels/')
    image_ids = open('data/ImageSets/%s.txt' % (image_set)).read().strip().split()
    list_file = open('data/%s.txt' % (image_set), 'w')
    for image_id in image_ids:
        list_file.write('data/images/%s.jpg\n' % (image_id))
        convert_annotation(image_id)
    list_file.close()
```
## 配置data和names文件
新建helmet.data,添加如下内容
```
classes=2
train=data/train.txt
valid=data/test.txt
names=data/rbc.names
backup=backup/
eval=coco
```
新建helmet.names,添加如下内容
```
hat
person
```
## 修改配置文件
修改cfg目录下的你要用到的cfg文件，此处以yolov3-tiny.cfg为例

找的yolo关键字，将附近的classes和filters修改一下

把classes改为你要检测的类数

将filters改为3*(classes + 4 + 1)，我的classes为2，所以我的filters为21
``` 
[net]
# Testing
batch=1
subdivisions=1
# Training
# batch=64
# subdivisions=2
width=416
height=416
channels=3
momentum=0.9
decay=0.0005
angle=0
saturation = 1.5
exposure = 1.5
hue=.1

learning_rate=0.001
burn_in=1000
max_batches = 500200
policy=steps
steps=400000,450000
scales=.1,.1

[convolutional]
batch_normalize=1
filters=16
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=32
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=64
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=128
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=256
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=2

[convolutional]
batch_normalize=1
filters=512
size=3
stride=1
pad=1
activation=leaky

[maxpool]
size=2
stride=1

[convolutional]
batch_normalize=1
filters=1024
size=3
stride=1
pad=1
activation=leaky

###########

[convolutional]
batch_normalize=1
filters=256
size=1
stride=1
pad=1
activation=leaky

[convolutional]
batch_normalize=1
filters=512
size=3
stride=1
pad=1
activation=leaky

[convolutional]
size=1
stride=1
pad=1
filters=21
activation=linear



[yolo]
mask = 3,4,5
anchors = 10,14,  23,27,  37,58,  81,82,  135,169,  344,319
classes=2
num=6
jitter=.3
ignore_thresh = .7
truth_thresh = 1
random=1

[route]
layers = -4

[convolutional]
batch_normalize=1
filters=128
size=1
stride=1
pad=1
activation=leaky

[upsample]
stride=2

[route]
layers = -1, 8

[convolutional]
batch_normalize=1
filters=256
size=3
stride=1
pad=1
activation=leaky

[convolutional]
size=1
stride=1
pad=1
filters=21
activation=linear

[yolo]
mask = 0,1,2
anchors = 10,14,  23,27,  37,58,  81,82,  135,169,  344,319
classes=2
num=6
jitter=.3
ignore_thresh = .7
truth_thresh = 1
random=1
```
## 开始训练
``` bash
python train.py --data-cfg data/helmet.data --cfg cfg/yolov3-tiny.cfg --epochs 10
```

## 预测
将你要预测的图片移动到sample目录下，执行
```
python detect.py --data-cfg data/helmet.data --cfg cfg/yolov3-tiny.cfg --weights weights/best.pt
```

## 我配置好的项目
地址：http://121.199.32.198/mirrors/yolov3.zip  
## 数据集
百度云下载地址：https://pan.baidu.com/s/1UbFkGm4EppdAU660Vu7SdQ

我的镜像站备用地址：http://121.199.32.198/mirrors/VOC2028.zip

**具体教程请去GitHub看作者提供的文档**
>参考博客：https://blog.csdn.net/public669/article/details/98020800  
>GitHub项目地址:https://github.com/ultralytics/yolov3