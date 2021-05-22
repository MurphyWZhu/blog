---
title: "PostgreSQL安装"
date: 2021-05-21T19:35:00+08:00
tags: ["数据库", "postgres"]
draft: true
---

### `PostgreSQL`

`postgres`是一款开源数据库

### `PostgreSQL`的安装

#### `Linux`下安装

`Debian`

```
```

`CentOS`

```
```

`alpine`

```
sudo apk add postgresql
sudo rc-update add postgresql boot
sudo reboot
```

#### `Windows`下安装

下载`PostgreSQL`安装程序

![postgres_win_diwnload](https://mublog.oss-cn-beijing.aliyuncs.com/postgres_win_download.png)

下载完成后运行，按提示安装即可

<img src="https://mublog.oss-cn-beijing.aliyuncs.com/postgres_win_install0.png" alt="postgres_win_install0" style="zoom: 80%;" />

<img src="https://mublog.oss-cn-beijing.aliyuncs.com/postgres_win_install1.png" alt="postgres_win_install1" style="zoom: 80%;" />

<img src="https://mublog.oss-cn-beijing.aliyuncs.com/postgres_win_install2.png" alt="postgres_win_install2" style="zoom: 80%;" />

安装完成后，在开始菜单运行`psql shell`，即可连接服务器

<img src="https://mublog.oss-cn-beijing.aliyuncs.com/postgres_win.png" alt="postgres_win" style="zoom: 80%;" />

#### `MacOS`下安装

#### 使用容器安装
