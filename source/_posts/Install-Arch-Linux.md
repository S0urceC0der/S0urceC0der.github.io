title: Arch Linux安装教程
date: 2014-11-22 22:29:33
tags: ["Linux"]
---

## 缘起

最早接触Arch Linux是在[@layerssss](https://github.com/layerssss/)推荐下使用的。相比于其他的Linux系统（之前曾经用Ubuntu、Backtrack/Kali Linux）而言，有以下的特点：

- 软件版本较新：所有的软件都进行滚动更新，基本是最新版软件，而且有很多都直接是Git包。

- 软件自定义安装：默认的系统是最精简的配置，不会有额外的东西。

- 软件非常丰富：加上aur库，基本能够包含各种的软件了。

这些优点也对应了一些缺点

- 经常会出现最新软件依赖问题导致无法更新

- 各种配置特别容易出错。

## 安装过程

之前安装过Kali Linux，但是因为添加了SSD硬盘，因此电脑的分区有所变化，需要将Windows安装到SSD上。之前的Kali Linux安装在机械硬盘上，分区分的也比较小，也就想趁此机会把分区调大一点，结果在调整/分区的时候直接出错，不得不重装。

### 安装基本环境

刻录一个Arch Linux启动U盘，然后引导进入后，主要参考[Wiki](https://wiki.archlinux.org/index.php/Beginners%27_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))来进行安装，安装步骤基本一致，需要注意的地方如下

0. 通用恢复方案

貌似Arch Linux的Grub2引导菜单中没有恢复模式（抱歉不知道另外一个是什么功能），一般情况下如果有问题可以通过Live CD/USB来启动，我一开始比较笨的方法就是启动Live CD后，然后arch-chroot到对应的路径，进行系统修改操作。也可以直接进恢复模式。

1. Wifi连接

安装过程中全程使用Wifi进行连接，直接使用`wifi-menu`进行每次的连接，但是安装完新系统后，还需要额外安装`dialog`和`wpa_supplicant`

### 显卡驱动问题

这是第一个大坑，因为我的笔记本显卡是9300M GS，目前已经[不再被官方支持](https://www.archlinux.org/news/nvidia-340xx-and-nvidia/)，需要安装nvidia-340xx包。这个修改是在10月份的才发生的，因此在对应的中文Wiki并没有修改过来，直接导致多次安装仍然出问题。

安装的时候最好一开始就选择安装Nvidia的私有显卡驱动，而不安装Nouveau，虽然Wiki上有如何[切换Nouveau和Nvidia](https://wiki.archlinux.org/index.php/NVIDIA#Switching_between_NVIDIA_and_nouveau_drivers)，但是实际切换的时候没有成功。

安装完毕后Xorg可以安装XTerm以测试Xorg和显卡驱动是否正常。

### 分区挂载

Live CD能够正常进行挂载NTFS分区，但是如果直接在Live CD中挂载后生成为fstab，则无法直接启动挂载

需要安装[ntfs-3g](https://wiki.archlinux.org/index.php/NTFS-3G)，并利用其进行挂载，并手工修改对应的fstab文件

### 字体渲染

Arch Linux的默认字体渲染极差，据说Linux上的字体渲染效果会好于Windows（显然不会好于MacType），但是需要相对复杂的调整，目前我也没有达到非常好的字体渲染效果。

为了提升字体配置效果，首先要安装一些软件包，包括fontconfig-infinality。也有利用Ubuntu配置文件的一些包例如fontconfig-ubuntu-zh-cn（安装的时候密钥更新失败）。

我的配置如下，Arch Linux默认没有生成字体配置文件，首先参考官方的[字体配置Wiki](https://wiki.archlinux.org/index.php/Font_Configuration_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29)，生成一个默认的配置文件。然后进入[在线字体配置](http://wenq.org/cloud/fcdesigner_local.html)选择一个合适的配置，我选择了DejaVu作为英文字体，文泉驿微米黑作为中文字体，

## 总结

以上是安装Archlinux的安装过程，记录了遇到的主要问题，希望再次安装Arch Linux时可以依靠这个来减少搜索的花费。当然，同时也仍然有很多问题没有解决。

## 其他问题

1. Arch Linux Wiki最近访问不稳定，时不时的无法访问。

2. Arch Linux在时间较长后进入睡眠（或者休眠？）后会出现无法进入系统的情况。

3. 字体渲染仍然要进一步优化。

发现有一个网页[一条命令搞定Linux字体渲染——Ubuntu系发行版微软雅黑+宋体终极解决方案【原创推荐】 ](http://www.lulinux.com/archives/278)可以参考一下。

## 更新

#### [2015-01-03]

更新系统时，遇到错误

> 错误：无法准备事务处理 (无法满足依赖关系)
> :: package-query: 要求 pacman<4.2

后来参照文章[Archlinux 升级 pacman 时遇到的问题及其解决](http://www.cnblogs.com/ccpaging/p/4191592.html)解决
