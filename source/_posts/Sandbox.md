title: "搭建Cuckoo沙盒测试环境"
date: 2015-04-25 16:34:13
tags:
---

Cuckoo是一款用于自动化分析恶意样本的沙箱软件。相比其他沙箱软件，Cuckoo具备强大的监控功能，能够自动将程序提交到指定的虚拟机中，并依据格式运行，同时会Hook相关的函数接口，记录下的重要的函数调用、网络访问。Cuckoo基于Python编写，通过在此基础上进行修改，可以避免重复开发，增加所需要的特殊功能。

网站https://malwr.com/可以认为是一个在线版的Cuckoo，国内也样相关的在线样本行为分析网站[火眼](https://fireeye.ijinshan.com/)和[文件B超](https://www.b-chao.com)，个人怀疑他们也是在Cuckoo的基础上进行修改的。

本文的主要内容是在Linux系统上安装Cuckoo。

## 安装过程

### 1. 基本运行环境

* Python

安装Python并根据requirements.txt中的列表直接安装所需要的python模块。

* MongoDB

如果想使用基于Django的网页接口，则需要安装MongoDB

* 各个虚拟机的接口库

KVM、XenServer的API接口

### 2. 功能软件

#### 安装Yara

Yara是一款用于识别和分类恶意样本的开源工具，可以理解为是一款特征匹配工具。

Yara默认没有编译对cuckoo和magic的支持，在编译时需要加上`./configure --enable-cuckoo --enable-magic`选项。

安装yara-python库，使得cuckoo能够调用yara。

#### 安装ssdeep

ssdeep能够计算一个文件的模糊哈希代码，通过计算不同样本之间的ssdeep差值，可以判断两个样本是否相近。

#### Tcpdump

用于截获数据。

#### 安装cuckoo

cuckoo居然没有在ArchLinux的软件包中，这是很神奇的一件事，所以必须手动Github下载，并且手动更新。

`git clone git@github.com:cuckoobox/cuckoo.git`

### 3. 可选软件

### Volatility

这是一款分析内存dumps的工具。

### 4. 安装虚拟机

Cuckoo支持多种虚拟机，官方推荐使用的是VirutalBox虚拟机（应该是因为开源的缘故），其他的还支持VMware、XenServer、ESX、KVM等虚拟机（参考modules/machinery目录），个人处于使用惯性，选择了使用VMware。

虚拟机内安装基本操作系统，取消防火墙，并安装Python运行环境，然后运行agent/agent.py，使得虚拟机处于等待接收参数和处理任务状态，并对此状态保存快照。

## 配置

#### cuckoo.conf

[cuckoo]machinery: 虚拟机类型，可以填virutalbox，vmware等

[resultserver]ip port: 存储结果的服务器IP，可以直接填写本机（物理机）

[database]connection: 数据库连接，不填可以直接使用SQLite数据库

#### auxiliary.conf

主要用于监控网卡数据相关配置

[sniffer]interface: 改为和虚拟机共用的网卡

#### vmware.conf

具体的虚拟机设置，本例中使用vmware.conf

machines: 具体的虚拟机配置，必须与下面的字段名称一致

vmx_path: 表示虚拟机vmx文件的路径

snapshot: 快照名称

ip: 虚拟机IP地址

#### processing.conf

处理样本时需要使用各种附加模块，我禁用了Virustotal。此外，如果自己添加了模块，则需要在这个文件中启用

#### memory.conf

用于Volatility处理模块，需要首先在processing.conf启用memory模块才行。

#### reporting.conf

配置报告服务器，我修改了mongodb字段，使其将数据存储到了MongoDB中

## 运行

### 启动cuckoo

`python2 cuckoo.py`

如果失败，可以使用-d参数，输出更多的内容用于调试确认错误信息。

### 提交文件分析

`python2 utils/submit.py [文件路径] --url [网址]`


