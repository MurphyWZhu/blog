---
title: "Typora设置等宽字体"
date: 2021-05-08T20:13:33+08:00
draft: false
---

我在ArchLinux上用Typora的时候发现代码块字体不是等宽字体，就非常的难受

![01](https://murphyblog.oss-cn-beijing.aliyuncs.com/Screenshot_20201127_144511.png)

在网上搜索之后，找到了解决办法

新建这个文件~/.config/Typora/themes/base.user.css

写入以下内容，字体可以替换为喜欢的字体

```css
~/.config/Typora/themes/base.user.css
.CodeMirror-wrap .CodeMirror-code pre {
    font-family: Fira Code;
}
.md-fences,code,tt {
    font-family: Fira Code;
}
```

![02](https://murphyblog.oss-cn-beijing.aliyuncs.com/Screenshot_20201127_144616.png)

保存重启就好了

![03](https://murphyblog.oss-cn-beijing.aliyuncs.com/Screenshot_20201127_144643.png)

