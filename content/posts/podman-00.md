---
title: "在Archlinux上安装Podman"
date: 2021-05-15T12:36:01+08:00
draft: false
tags: ["容器","podman"]
---

### `Podman`

`podman`是一个无守护进程的容器引擎，它还可以实现无根运行容器，现在最新版本的红帽系Linux都默认使用`podman`。

### 在`Archlinux`上安装`podman`

安装`podman`

```bash
sudo pacman -S podman crun
```

配置无根运行

```bash
sudo touch /etc/subuid /etc/subgid
```

```bash
sudo usermod --add-subuids 100000-165536 --add-subgids 100000-165536 username #将username替换为用户名
```

查看`podman`信息

```
podman info
```

### 配置`podman`镜像仓库

新建`registries.conf`配置文件

```bash
mkdir -p $HOME/.config/containers
vim $HOME/.config/containers/registries.conf
```

将以下内容写入`registries.conf`

```yaml
unqualified-search-registries = ["docker.io"]

[[registry]]
prefix = "docker.io"
location = "docker.mirrors.sjtug.sjtu.edu.cn"
```

查看是否成功

```bash
podman info | grep Location
输出：Location: docker.mirrors.sjtug.sjtu.edu.cn
```

