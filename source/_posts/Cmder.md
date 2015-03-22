title: "Cmder"
date: 2015-03-21 15:34:12
tags:
---

## Cmder简介

Cmder是一款Windows下面的终端模拟器，有了它终于能够抛弃默认的垃圾cmd界面了！

![Cmder](/img/cmder.png)

### 特点

个人很简单的使用了下，觉得有以下优点：

1. 可自定义字体，配色非常好看。

2. 路径补全有所提高。

## 使用配置

### 乱码

ls的乱码主要是参数问题，修改文件cmder/config/aliases：

```
l=ls --show-control-chars
la=ls -aF --show-control-chars
ll=ls -alF --show-control-chars
ls=ls --show-control-chars -F
```

### 中文重叠

在设置中，取消`Main console font`下的Monospace勾，这样中文字符（占用两个英文字符空间）就得到的了足够的空间了。

### 右键打开Cmder

以管理员权限运行，输入命令`.\cmder.exe /REGISTER ALL`，在任意目录下右击就会出现`Cmder Here`，可以打开并默认切换到该目录。

## 不足

1. 有时候会出现无法删除第一个字符的情况（实际已经删除了）。

2. 路径补全还是没有达到zsh的效果。

3. 没有快速进入某个目录的功能。

## 最后

现在只是刚开始使用，很多功能还是在摸索中，如果有更多的体会，会继续更新。
