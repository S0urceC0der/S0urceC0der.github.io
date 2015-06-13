title: "Cuckoo代码学习一"
date: 2015-06-13 20:18:40
tags:
---

## 虚拟机监控部分

Agent

主要功能：

使用SimpleXMLRPCServer建立一个服务器监听Cuckoo发送的数据。

dll注入

使用了远程DLL注入技术，

参考文档[远程DLL注入技术](http://www.programlife.net/remote-thread-dll-injection.html)

监控
