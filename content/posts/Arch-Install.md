---
title: "Arch Install"
date: 2021-04-21T12:49:02+08:00
draft: false
cover:
    image: "https://mublog.oss-cn-beijing.aliyuncs.com/824c2c178b82434c2abbe9e485de1daa.jpeg"
    alt: "ArchLinux"
    # caption: "ArchLinux"
    relative: false # To use relative path for cover image, used in hugo Page-bundles
---

### 安装前准备

#### 制作启动盘

下载最新版本的[`archlinux`](https://mirrors.bfsu.edu.cn/archlinux/iso/latest/)安装镜像文件

##### `Linux`：

```bash
dd bs=4M if=镜像路径 of=/dev/USB设备标识符 conv=fsync oflag=direct status=progress
```

##### `Windows`：

使用[`Rufus`](https://rufus.ie/zh/)制作

##### `MacOS`：

```bash
diskutil unmountDisk /dev/USB设备标识符 #MacOS也许会自动挂载USB设备，先卸载
```

```bash
dd if=镜像路径 of=/dev/USB设备标识符 bs=1m
```

#### 引导系统

将制作好的`USB`启动盘插在要安装`archlinux`的电脑上

开机的时候按某个按键(不同品牌及机器不一样)，进入选择引导介质的界面，选择`USB`设备

即可引导进入`archlinux live`

------

### 安装前配置

```bash
setfont /usr/share/kbd/consolefonts/iso01-12x22.psfu.gz #将字体设置大一点
timedatectl set-ntp true #设置网络时间
```

#### 磁盘设置

> 数据无价，请确认后再操作

##### 分区

`archlinux`一般需要分一下几个区

|    分区名称    |   挂载点    |        分区类型        |    建议大小    |
| :------------: | :---------: | :--------------------: | :------------: |
| `EFI`引导分区  | `/mnt/boot` | `EFI system partition` |     `512M`     |
|     根分区     |   `/mnt`    |        `Linux`         | 磁盘剩下的空间 |
| 数据分区[可选] |   `/data`   |        `Linux`         |                |
|  家分区[可选]  |   `/home`   |        `Linux`         |                |

分区要具体看用途，及你的电脑的磁盘配置

如果只有一块硬盘，可以只分`EFI`分区及根分区

如果有一块固态和一块机械，可以把固态分为`EFI`分区及根分区，机械分为数据分区或家分区

我的电脑是一块`NVME`固态和一块`SATA`固态，我将`NVME`分为`EFI`分区及根分区，将`SATA`分为家分区

计划好分区后使用`fdisk`命令来分区

使用`fdisk /dev/磁盘标识符`进入交互式工具

可以使用`lsblk`查看到磁盘标识符

如我的电脑有一块名为`sda`的`sata`固体硬盘和一块名为`nvme0n1`的`nvme`固体硬盘

```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda           8:0    0 465.8G  0 disk 
└─sda1        8:1    0 465.8G  0 part /home
nvme0n1     259:0    0 238.5G  0 disk 
├─nvme0n1p1 259:1    0   512M  0 part /boot
└─nvme0n1p2 259:2    0   238G  0 part /
```

我们主要使用`fdisk`的以下功能

查看分区，在`fdisk`输入`p`即可查看已有分区的信息

新建分区表，在`fdisk`输入`g`即可创建一个新的`gpt`分区表

新建分区，在`fdisk`输入`n`并根据提示输入大小等信息即可创建一个分区

更改分区类型，`fdisk`默认的分区格式是`linux`，创建`EFI`需要将分区格式改为`EFI`格式，在`fdisk`输入`t`，再根据提示输入`EFI`的代码`1`，即可更改为`EFI`类型，只需要更改`EFI`分区的格式就行了

保存更改，`fdisk`交互式命令的操作不会马上生效，需要保存才能生效，输入`w`即可保存**(数据无价请确认后再操作)**

##### 格式化分区

`EFI`分区：

```bash
mkfs.fat -F32 /dev/nvme0n1p1 #数据无价请确认后再操作
```

根分区：

```bash
mkfs.ext4 /dev/nvme0n1p2 #数据无价请确认后再操作
```

数据分区或家分区：

```bash
mkfs.ext4 /dev/sda1 #数据无价请确认后再操作
```

##### 挂载分区

```bash
mount /dev/nvme0n1p2 /mnt
mkdir /mnt/boot
mkdir /mnt/home
mount /dev/nvme0n1p1 /mnt/boot
mount /dev/sda1 /mnt/home
```

#### 修改镜像站

为了加快下载速度，修改镜像站

修改文件`/etc/pacman.d/mirrorlist`

```apacheconf
Server = https://mirrors.bfsu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

------

### 安装基本系统

```bash
pacstrap /mnt base base-devel linux-zen linux-firmware vim networkmanager iwd iptables-nft
```

`base`和`base-devel`系统的一些最基本的程序，`linux-zen`是添加修改了一下功能的`linux`内核，`iwd`是`intel`无线网卡驱动，`iptables-nft`是使用 `nft` 接口的`iptables`

将磁盘挂载信息写入新系统

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

------

### 配置系统

#### 切换到新系统

```bash
arch-chroot /mnt
```

#### 本地化设置

设置时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

设置中文支持，修改`/etc/locale.gen`

```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
zh_TW.UTF-8 UTF-8
zh_HK.UTF-8 UTF-8
```

使配置生效

```
locale-gen
```

先将语言设置为英文`utf8`，先设置成英文，中文在字符界面会乱码

```apacheconf
/etc/locale.conf
LANG=en_US.UTF-8
```

#### 网络配置

配置主机名

```apacheconf
/etc/hostname
karch
```

配置`hosts`文件

```apacheconf
/etc/hosts
# Static table lookup for hostnames.
# See hosts(5) for details.
127.0.0.1	localhost
::1		localhost
127.0.1.1	karch.localdomain	karch
```

#### 其他设置

##### 设置`root`密码

```bash
passwd
```

##### 安装`intel cpu`微码（或`amd-ucode`）

```bash
pacman -S intel-ucode
```

#### 引导设置

配置`grub`启动

```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

#### 退出新系统

```
exit
```

------

### 重启电脑

在重启电脑前先先将挂载的磁盘卸载

```
umount /mnt/boot
umount /mnt/home
umount /mnt
```

重启电脑

```
reboot
```

### 必要设置

#### 网络配置

启动`NetworkManager`网络服务

```
systemctl enable NetworkManager
```

连接网络

配置`dkms`和显卡驱动(`Nvidia`独显+`Intel`集显)

```bash
sudo pacman -S linux-zen-headers dkms
sudo pacman -S nvidia-dkms nvidia-prime
sudo pacman -S vulkan-intel intel-media-driver libva-intel-driver 
```

安装`kde`桌面环境，以及`pipewire`音频管理，和一些常用的软件

```bash
sudo pacman -S plasma konsole dolphin{,-plugins} pipewire{,-alsa,-pulse,-jack} ark gwenview kdialog spectacle kate 
```

安装中文字体和常用字体并设置默认语言为中文

```bash
sudo pacman -S ttf-sarasa-gothic ttf-fira-{code,mono}
/etc/locale.confLANG=en_US.UTF-8
```

安装`libreoffice`办公套件

```bash
sudo pacman -S libreoffice-fresh{,-zh-cn} okular latte-dock kfind krename kcalc elisa vlc
```

配置`archlinuxcn`源

```bash
sudo pacman -S haveged
sudo systemctl start haveged
sudo systemctl enable haveged
sudo vim /etc/pacman.conf
sudo rm -rf /etc/pacman.d/gnupg
sudo pacman-key --init
sudo pacman-key --populate archlinux
sudo pacman -S archlinuxcn-keyring
```

安装`chrome`浏览器

```bash
pacman -S google-chrome
```

安装输入法

```bash
pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-nord fcitx5-pinyin-zhwiki fcitx5-pinyin-moegirl
~/.pam_environmentGTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```
