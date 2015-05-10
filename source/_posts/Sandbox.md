title: "搭建沙盒测试环境"
date: 2015-04-25 16:34:13
tags:
---

## 安装过程

主要目标是搭建Cuckoo沙箱环境

### 安装VirtualBox

需要手动添加Virtualbox源。

### 安装ssdeep

### 安装Yara

Yara默认没有编译对cuckoo和magic的支持。

Yara安装cuckoo支持时，需要Jansson库，但是Debian库中的libjansson-dev版本比较旧，需要手动下载最新版。

安装yara-python库。

### Volatility

对于Kali Linux可以直接安装，也可以手工安装。

### 安装cuckoo

`git clone git@github.com:cuckoobox/cuckoo.git`

## 配置

### 虚拟机

取消防火墙，安装python环境

后台运行client.py接收参数传递

## 使用

-d参数可以显示更多的内容。


