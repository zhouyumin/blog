---
title: Hexo+Gitee建立blog
date: 2020-01-17 10:23:11
categories:
  - hexo
tags: 
  - hexo
  - gitee
  - blog
---

## 安装依赖环境

**1.安装Git**

去[官网](https://git-scm.com/downloads)下载安装包,如果嫌弃[Git官网](https://git-scm.com/downloads)下载慢的话就去国内镜像站下载，这里推荐[淘宝镜像站](https://npm.taobao.org/mirrors/git-for-windows/)选择需要的版本下载安装，Linux用户则可用以下命令快速安装。
``` bash
$ sudo apt install git
```
如果你不了解Git可阅读[Git简单入门](/2020/01/18/Git简单入门/)

**2.安装nodejs,npm**

去nodejs的[中文官网](http://nodejs.cn/download/)下载安装，Windows版应该自带npm包管理工具。

Linux命令下载
``` bash
$ sudo apt install nodejs
$ sudo apt install npm
```
---
<!-- more -->
## 安装Hexo
打开终端输入 `npm install -g hexo`就能安装hexo了

如果Linux用户有权限问题则使用`sudo npm install -g hexo`

如果你嫌下载慢的话可以给npm换淘宝镜像源（换源大法好）请参考[npm换淘宝镜像源](https://www.jianshu.com/p/4aaf929bfa71)

---

## 初始化hexo

**1.首先建立你的blog文件夹**

``` bash
mkdir blog
```

**2.进入文件夹**

``` bash
cd blog
```

**3.初始化**

``` bash
hexo init
```

等待一会初始化完成后会在该目录下生成相应的文件

**4.修改相关配置**
编辑blog下的_config.yml文件，修改相关信息
``` yaml
# Site
title: 标题
subtitle: 副标题
description: 个人描述
keywords: 关键词
author: 作者
language: 语言 推荐zh-Hans
timezone: 时区 可以不用管
```

**5.下载主题**

这里推荐Next主题，功能多，简洁
``` bash
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
如果你嫌clone慢的话可以直接去该项目地址下载zip包解压到themes目录下，主题相关配置及功能开启请去[官网](http://theme-next.iissnan.com/getting-started.html)查看相关教程，这里就不再赘述。

**6.启用主题**

编辑编辑blog下的_config.yml文件
找到theme关键字，修改为
``` yaml
theme: next
```

当然你也可以使用其他主题

---
## 开始使用
**清除编译文件**
```bash
hexo clean
```
**hexo编译 | generate** 
``` bash
hexo g
```
**hexo 运行 | sever**
``` bash
hexo s
```
访问http://localhost:4000/ 就能看到本地blog的预览效果

**远程发布 | deploy**
``` bash
hexo d
```
这个命令等部署完会用到

---
## blog部署
**1.创建gitee账号**

访问 https://gitee.com/ ，申请注册账号，码云类似国内版的GitHub。

**2.创建项目**
新建仓库名称为blog，我这里已经建好了所以提示已存在  
这样你就能以 username.gitee.io/blog 访问你的blog，如果你不想以二级目录的形式访问blog想以username.gitee.io的形式直接访问，你可以把仓库名设置成和用户名一样

![创建仓库](/images/pictures/blog/create_repertory.jpg)

**3.修改_config.yml**
``` yaml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: 你的项目地址  #比如我填的是https://zym4732.gitee.io/blog/
root: 项目名称    #我的是/blog
```
``` yaml
deploy:
  type: git               #type为git
  repo: 你新建仓库的URL    #例如我的是https://gitee.com/zym4732/blog.git
  branch: master
```

**4.发布blog**

安装自动部署发布工具
``` bash
npm install hexo-deployer-git --save
```
发布
``` bash
hexo d
```
使用发布命令需要我们输入Gitee的用户名和密码

**5.启用Gitee Page**
点击服务选择 Gitee Pages

![启用Page](/images/pictures/blog/Gitee_Page1.jpg)

然后更新就能使用了，网址就是图中红框的那个

![更新Page](/images/pictures/blog/Gitee_Page2.jpg)

这样你的blog就部署完成了

---
## 新建文章

使用命令
``` bash
hexo n 文章名
```
会在source/_posts/目录下自动建立相应的md文件

不使用命令则可以手动在source/_posts/目录下新建md文件

编辑好md发布就行

---
***OVER***
