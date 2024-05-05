---
title: ArchLinux安装
description: 手把手的ArchLinux安装与配置
categories:
    - Linux
tags:
    - ArchLinux
---

<br/>

{% emoji blobcat ablobcatattentionreverse %} 

详细安装信息参考 [ArchLinux Wiki](https://wiki.archlinux.org/)

## 下载ArchLinux官方镜像

[ArchLinux官方网站](https://www.archlinux.org/)

## 制作启动U盘

启动U盘根据操作系统自行选择工具制作
- Win: UtralISO
- Mac: balena Etcher / dd命令
- Linux: dd命令

## 安装ArchLinux

重启UEFI引导进入ISO镜像分区

### 基础配置

#### 键盘

默认为美式键盘，国内的键盘大部分都是美式键盘，可以不用配置。

```bash
# 查看可使用的键盘布局
ls /usr/share/kbd/keymaps/**/*.map.gz
# 加载指定的键盘布局
loadkeys xxx
```

#### 网络

- 有线网：路由器直连无需配置，网络自动联通
- 无线网：参考[iwctl](https://wiki.archlinux.org/index.php/Iwd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

#### 时间

```bash
timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp true
```

### 配置磁盘

1. 查看当前的硬盘分区信息

```bash
lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0 465.8G  0 disk                # 我的硬盘
├─nvme0n1p1 259:1    0    99M  0 part                # Win10 EFI分区
├─nvme0n1p2 259:2    0   128M  0 part                # Win10 不造啥分区
├─nvme0n1p3 259:3    0  99.4G  0 part                # Win10 C盘
├─nvme0n1p4 259:4    0   577M  0 part                # Win10 不造啥分区
├─nvme0n1p5 259:5    0    50G  0 part                # Win10 D盘
├─nvme0n1p6 259:6    0 215.5G  0 part                # Win10 E盘
├─nvme0n1p7 259:7    0   512M  0 part /boot/EFI      # ArchLinux EFI分区  -- 即将创建的分区
├─nvme0n1p8 259:8    0    40G  0 part /              # ArchLinux 根分区  -- 即将创建的分区
└─nvme0n1p9 259:9    0  59.5G  0 part /home          # ArchLinux home分区  -- 即将创建的分区
```

2. 创建分区

- 若安装盘为一整个硬盘，可以直接删除磁盘上的所有残留分区，重新进行分区
- 若安装盘为一个硬盘的部分（比如我的硬盘已经安装了Win10），则根据情况删除不需要的分区或者使用空闲空间（空闲空间可以从Win10上压缩一部分）

最终创建上述3个分区即可，其中EFI分区大小512M，其余两个根据需要配置，根分区不小与20G

```bash
fdisk /dev/nvme0n1  # 用法自己查
```

3. 格式化分区

```bash
mkfs.vfat /dev/nvme0n1p7  # EFI分区格式为vfat
mkfs.ext4 /dev/nvme0n1p8  # 根分区格式为ext4
mkfs.ext4 /dev/nvme0n1p9  # 家分区格式为ext4
```

4. 挂载分区

```bash
mount /dev/nvme0n1p8 /mnt
mkdir -p /mnt/boot/EFI
mkdir -p /mnt/home
mount /dev/nvme0n1p7 /mnt/boot/EFI
mount /dev/nvme0n1p9 /mnt/home
```

### 配置Pacman源

修改`/etc/pacman.d/mirrorlist`在软件源列表最上方添加国内的源

- 163： http://mirrors.163.com/archlinux/$repo/os/$arch
- 清华： http://mirrors.tuna.tsing.edu.cn/archlinux/$repo/os/$arch

### 安装基础操作系统

```bash
pacstrap /mnt base linux linux-firmware
```

### 生成分区表

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### 切换根分区

```bash
arch-chroot /mnt
```

### 时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

### 语言

编辑`/etc/locale.gen`, 去掉`en_US.UTF-8`的注释

```bash
locale-gen
```

编辑`/etc/vconsole.conf`，添加`LANG=en_US.UTF-8`

### 修改root密码

```bash
passwd
```

### 安装必须的软件包

使用`pamman`安装如下软件包
CPU
```bash
pacman -S xxx xxx xxx ...
```

| 软件包或软件包组(我总觉得不太全，看情况自己装叭) |
| :--------------: |
|       vim        |
|    base-devel    |
|      dhcpcd      |
|    net-tools     |
|     dnsutils     |
|    inetutils     |
|     iproute2     |
|  NetworkManager  |
|   intel-ucode    |

### 配置引导

```bash
# 安装引导软件
pacman -S grub efimanager os-prober
# 如果你有Windows
mkdir /boot/EFI/EFI-WIN
# 挂载Windows的EFI分区
mount /dev/nvme0n1p1 /boot/EFI/EFI-WIN
# 生成引导
grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootleader-id=ArchLinux
# 生成启动配置
grub-mkconfig -o /boot/grub/grub.cfg
```

### 退出重启

```bash
exit
umount -R /mnt
reboot
```

至此ArchLinux安装完成，重启后进入grub界面，选择ArchLinux进入archlinux，选择windows boot manager进入Windows

## KDE

### 创建用户

```bash
groupadd mygroup
useradd myname -m -d /home/myname 
passwd myname
```

### 安装KDE

```bas 
# 基础包
pacman -S xorg xorg-serve
pacman -S sddm kde-applications sddm-kcm
#（可选）集显驱动
pacman -S xf86-video-intel  #intel
pacman -S xf86-video-ati    #amd
#（可选）Nvidia驱动
pacman -S nvidia
# 声音
pacman -S alsa-utils pulseaudio pulseaudio-alsa

# 自启动
systemctl enable sddm
systemctl enable NetworkManager
systemctl enable dhcpcd
```

### 配置

#### 输入法

```bash
pacman -S fcitx fcitx-im fcitx-configtool 
yay -S fcitx-sogoupingyin

vim /home/myusername/.xprofile
# 添加
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

#### oh-my-zsh

```bash
pacman -S zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

主题`ys`

#### neofetch

```bash
pacman -S neofetch
```

#### 全局主题

使用WhiteSur

#### dock面板

```bash
pacman -S latte-dock
``` 

## 最终效果

{% image pingmu1.png 系统配置 %}

{% image pingmu2.png 桌面 %}
