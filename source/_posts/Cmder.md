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

### 退格后错位

> [lgh](https://www.v2ex.com/member/lgh):
> cmder 功能强大，但是反应较慢，经常卡住半天才反应；默认的命令提示符 λ 还会导致在用 TAB 还是 BACKSPACE 之后字符错位（自己改配置换成 $ 后就不会）。
> 现在干脆直接用 ConEmu 了。
> 来源：https://www.v2ex.com/t/155058

lgh针对问题1提出解决方法是修改提示符为$。修改方法是

打开文件config/prompt.lua，将第二行中的λ修改为Linux下常用的$

## 不足

当然，可能这些问题只是因为我没有找到对应的解决方法。

1. <del>有时候会出现无法删除第一个字符的情况（实际已经删除了）。</del>

2. 路径补全还是没有达到zsh的效果。

3. 没有快速进入某个目录的功能。

4. Cmder1.2开始需要VS**2015**运行库支持（丧心病狂！）

## 最后

现在只是刚开始使用，很多功能还是在摸索中，如果有更多的体会，会继续更新。
