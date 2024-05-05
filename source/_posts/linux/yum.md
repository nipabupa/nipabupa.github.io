---
title: "YUM软件包管理"
description: RedHat系Linux系统软件管理软件
categories:
    - Linux
tags:
    - Linux包管理
---

## 基本概念

yum是一个软件包管理工具

- Fedora、RedHat以及CentOS中使用
- 基于RPM包
- 从yum源上自动下载软件包
- 自动处理依赖性关系
- 一次安装所有依赖的软件包


## 常用命令

```bash
# 安装软件包
yum install xxx
# 卸载软件包
yum remove xxx
# 清理yum源本地缓存
yum clean all
# 重新生成yum源本地缓存
yum makecache
```

## yum源

### 配置

配置目录 `/etc/yum.repos.d/`，该目录下每一个`.repo`文件都被识别为yum源的配置，可以配置多个yum源

```repo
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
baseurl=http://mirrors.163.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
baseurl=http://mirrors.163.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
baseurl=http://mirrors.163.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://mirrors.163.com/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7
```

### 使用ISO作为yum源

1. 将ISO挂载到光驱
2. 将光驱挂载到目录
```bash
mkdir /root/iso
mount /dev/sr0 /root/iso
```
3. 修改yum配置文件
```bash
vim /etc/yum.repos.d/local.repo

[local]
name=local
baseurl=file:///root/iso/
gpgcheck=0
enabled=1
```
4. 生成yum源缓存
```bash
yum clean all
yum makecache
```
