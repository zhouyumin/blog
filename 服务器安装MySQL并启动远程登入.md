---
title: 服务器安装MySQL并启动远程登入
date: 2020-05-27 11:00:09
tags: 
	- MySQL
categories: 数据库
---

## 安装MySQL

``` shell
sudo yum install mysql-server
```

如果上述命令无法安装，原因是源内没有mysql

去官网下载MySQL yum Repository安装就行 官网地址：www.mysql.com

Repository下载地址： https://dev.mysql.com/downloads/repo/yum/

下载rpm后使用yum安装rpm，然后执行上述安装MySQL命令

安装rpm 命令

``` shell
sudo yum install -y mysql80-community-release-el7-3.noarch.rpm
```

## 设置开机自启

``` shell
sudo systemctl enable --now mysqld
```

## 检查启动状态

``` shell
sudo systemctl status mysqld
```

<!-- more -->

## 设置密码

``` shell
sudo mysql_secure_installation
```

要求你配置VALIDATE PASSWORD component（验证密码组件）： 输入y ，回车进入该配置
选择密码验证策略等级， 我这里选择0 （low），回车
输入新密码两次(大、小写字母，数字，符号都要有)
确认是否继续使用提供的密码？输入y ，回车
移除匿名用户？ 输入y ，回车
不允许root远程登陆？ 我这里需要远程登陆，所以输入n ，回车

其他默认y

## 实现远程

``` shell
use mysql;
update user set host = '%' where user = 'root';
```

## 刷新

``` shell
flush privileges;
```

## 重启服务

``` shell
sudo systemctl restart mysqld
```

## 服务器开放3306端口

去服务器的控制台安全组里配置规则添加3306端口

![](/images/pictures/port.jpg)

## 远程连接

![](/images/pictures/connect.jpg)