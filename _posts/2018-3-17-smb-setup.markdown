---
layout:     post
title:      "smb服务器 安装配置"
subtitle:   " \"smb server, smb setup\""
date:       2018-03-17 10:00:00
author:     "Shen"
header-img: "img/home-bg-art.jpg"
catalog: true
tags:
    - 计算机
    - 服务器
    - smb
    - Linux
---

> “Yeah It's on. ”


## 前言

昨天在京东上买了个2T的机械硬盘，之前电脑里面只有一个256G的固态硬盘，所以以前都是远程到大学时期的笔记本上的Linux，现在有机械硬盘，所以决定利用这个大容量的机械硬盘和win10自带的hyper-v虚拟机安装一个ubantu kylin。

## 安装Kylin

首先在启用和关闭window功能中打开Hyper-V；
开启这些功能需要重启电脑；
电脑重启后就能在所有应用中找到Hyper-V管理器;
打开Hyper-V管理器，接着新建虚拟机，选择下载好的iso文件。

## 安装smb服务器

1. 打开“终端窗口”，输入“sudo apt-get install samba samba-common”,按照提示输入密码，等待安装完成。

2. 打开"终端窗口"，输入"sudo mkdir /home/share"-->回车-->共享目录share新建成功。

3. 输入"sudo chmod 777 /home/share"-->回车,这样用户就对共享目录有了写权限。

4. 打开配置文件smb.conf，输入“security = user”
   [share] /* 共享名，不需要与共享目录同名 */  
   comment = my share directory /* 对共享目录的描述 */  
   path = /home/share /* 共享的目录 */  
   browseable = yes   /* 该共享可以浏览 */  
   writable = yes     /* 该共享目录可写 */  

5. 在终端中输入“sudo useradd smbuser”
   输入“sudo smbpasswd -a smbuser”
   然后重启smb服务，“sudo service smbd restart” 

—— Shen 后记于 2018.3