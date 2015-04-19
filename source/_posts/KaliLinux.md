title: "KaliLinux使用心得"
date: 2015-04-12 19:33:10
tags: [Linux]
---

## Kali Linux

Kali是一个专门用来进行安全审计（渗透测试）的操作系统，是著名的Backtrack的后继者，由同一组开发人员开发的。Kali基于Debian系统，加入了大量的安全工具。

## 基本配置

### 安装Linux Brew

### 允许使用PPA库

参考网页[Kali Linux add PPA repository add-apt-repository](http://www.blackmoreops.com/2014/02/21/kali-linux-add-ppa-repository-add-apt-repository/)

## 一些常用软件

### 安装Zsh、Terminator

用于替换原有的bash、Gnome Terminal。

### 安装VMware

Vmware Workstation的驱动对于不同的Linux内核版本都要进行一定的patch才能正常编译。基本上通过搜索能够获取最新需要修改的地方，然后重新打包后编译即可。
