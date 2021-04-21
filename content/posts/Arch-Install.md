---
title: "Arch Install"
date: 2021-04-21T12:49:02+08:00
draft: false
cover:
    image: "https://mublog.oss-cn-beijing.aliyuncs.com/824c2c178b82434c2abbe9e485de1daa.jpeg"
    alt: "ArchLinux"
    caption: "ArchLinux"
    relative: false # To use relative path for cover image, used in hugo Page-bundles
---

### 安装前配置

设置字体

```bash
setfont /usr/share/kbd/consolefonts/iso01-12x22.psfu.gz
```

连接网络

```
wifi:
iwctl
```

设置ntp

```
timedatectl set-ntp true
```

磁盘按照自己的需要分区

将根目录挂载到`/mnt`

修改镜像站

