---
title: SSH免密登入配置
date: 2020-07-29 20:58:51
tags:
	- Hadoop
categories: Hadoop
---

## SSH 介绍

ssh可以远程连接其他服务器

语法 `ssh username@ip`

例如 `ssh zym@hadoop100`

其中能直接写 `hadoop100` 是因为添加了IP映射  `hadoop100` 代表 `192.168.23.100`

<!-- more -->

## 无密码登入配置

### 生成公钥和私钥

输入命令

``` shell
ssh-keygen -t rsa
```

然后敲三下回车就会在  `~/.ssh` 目录中生成公钥和私钥

### 把公钥拷贝到要免密登入的服务器上

命令格式 `ssh-copy-id username@ip`

输入命令，下列命令省略了服务器的用户名，默认为当前用户名

``` shell
ssh-copy-id hadoop101
ssh-copy-id hadoop102
ssh-copy-id hadoop103
```

这样就把公钥拷贝到了集群服务器上了

其他服务器间实现免密登入同理，配置好密钥就行

如果要实现其他用户登入服务器免密，则更改 username 再配置一下密钥就行（原理都是相通的，得举一反三）

### .ssh文件夹下（~/.ssh）的文件功能解释

|     文件名      |                 描述                  |
| :-------------: | :-----------------------------------: |
|   known_hosts   | 记录ssh访问过计算机的公钥(public key) |
|     id_rsa      |              生成的私钥               |
|   id_rsa.pub    |              生成的公钥               |
| authorized_keys |    存放授权过得无密登录服务器公钥     |