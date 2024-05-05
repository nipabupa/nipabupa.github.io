---
title: RPM软件包
description: RedHat系Linux系统软件管理体系
categories:
    - Linux
tags:
    - Linux包管理
---

## 基本概念

- Red Hat Linux发行版专门用来管理Linux各项套件的程序
- CentOS、RedHat、Feora、Suse等多种linux发行版均使用
- 类似于windows的exe安装包

## 常用命令

```bash
# 安装rpm包
rpm -ivh xxx
# 升级rpm包
rpm -uvh xxx
# 卸载rpm包
rpm -e xxx
# 查看当前已安装的rpm包
rpm -qa
# 查看当前文件属于哪个rpm包
rpm -qf xxx
# 查看当前rpm包有哪些文件
rpm -qpl xxx
```
## 制作RPM包

- 源码包（资源）： xxx.tar.gz
- spec文件（规则）：xxx.spec


在`/root`目录下创建如下目录格式

```bash
rpmbuild
├── BUILD           # 构建目录
├── BUILDROOT       # 构建根目录
├── SRPMS           # 生成的src RPM包存放位置
├── RPMS            # 生成的RPM存放位置
├── SOURCES         # 源文件，补丁文件等存放位置
└── SPECS           # 构建目录
```

将spec脚本与源码包放在对应目录后，执行如下命令：

```bash
rpmbuild -ba xxx.spec
```

## Spce脚本

```spec
Name:
Summary:
Version:
Release:
Copyright:
Group:
License:
Source: Source.tar.gz
Patch:
Require:

%prep
rpm安装前执行的脚本

%preun
rpm卸载前执行的脚本

%setup
解压源码包

%patch
通常补丁都会一起在源码包中，或放到SOURCES目录下。

%build
编译源码构建包

%install
把软件安装到虚拟的根目录中

%clean
清除编译和安装时生成的临时文件

%post
rpm安装后执行的脚本

%postun
rpm卸载后执行的脚本

%files
文件段，用于定义构成软件包的文件列表，即哪些文件或目录会放入rpm中
```
