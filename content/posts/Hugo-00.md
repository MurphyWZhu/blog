---
title: "使用Hugo在github上搭建博客"
date: 2021-04-25T11:29:00+08:00
draft: false
---

> Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.

`Hugo`是一个静态网页框架，可以快速生成网页，并且拥有许多的主题

### 安装`Hugo`

在`archlinux`中

```bash
sudo pacman -S hugo
```

使用源码安装

需要有`git`和`go(>=1.11)`

```bash
mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install --tags extended
```



```
% hugo version  
hugo v0.82.0+extended linux/amd64 BuildDate=unknown
```

新建一个`hugo`网站项目

```
hugo new site blog -f yml
```

```
git init
git branch -m main
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1
git submodule update --init --recursive
git add *
git commit -m "first commit"
git remote add origin git@github.com:MurphyWZhu/blog.git
git push -u origin main
```

