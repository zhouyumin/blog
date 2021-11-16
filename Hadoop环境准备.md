---
title: Hadoop环境准备
date: 2020-07-29 18:29:01
tags:
	- Hadoop
categories: Hadoop
---

## 工具准备

- VMware Workstation Pro 虚拟机
- CentOS7 最小安装
- jdk-8u251
- Hadoop2.7.2

<!-- more -->

## 虚拟机配置

### 设置静态IP

`sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33`

增添或修改成下列配置

```shell
BOOTPROTO="static" #改为static 静态ip
IPADDR=192.168.23.100 #192.168.23要和虚拟机的网关前3段保持一致，第4段随意
NETMASK=255.255.255.0
GATEWAY=192.168.23.2
DNS1=192.168.23.2
```

网段要和虚拟机NAT设置里的网段一致

GATEWAY 和 DNS 要和 NAT 设置里的网关IP一致，NETMASK也要和 NAT 设置里的子网掩码一致

![](/images/pictures/net.png)

### 修改主机名

`sudo vim /etc/sysconfig/network`

添加如下内容

```shell
NETWORKING=yes
HOSTNAME=hadoop100 #设定的主机名
```

### 添加ip映射

`sudo vim /etc/hosts`

添加下列内容

```shell
192.168.23.100 hadoop100
192.168.23.101 hadoop101
192.168.23.102 hadoop102
192.168.23.103 hadoop103
```

### 物理机也要添加IP映射

同时也将上述内容添加到`C:\Windows\System32\drivers\etc\hosts`文件中

有权限无法修改请右键更改文件属性，修改文件所有者（具体不赘述）

### 关闭防火墙

防止端口无法访问，把防火墙关闭

```shell
sudo systemctl disable firewalld #永久关闭
sudo systemctl stop firewalld #暂时关闭
```

## 安装JDK和Hadoop

去官网下载相关压缩包，解压到指定目录下并添加PATH就行（具体不赘述）

## 更改文件所有者

为了避免文件权限问题，现在将 JDK 和 Hadoop 的所有者改为普通用户

命令

```shell
sudo chown -R whoami file #whoami 指当前登入的用户
```

例如

```shell
sudo chown -R whoami JDK #将JDK及其所有子文件的所有者改为当前登入用户， -R递归修改
sudo chown -R whoami Hadoop
```