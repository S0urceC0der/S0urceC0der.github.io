title: 从0开始学习Linux ROP利用-part2
date: 2018-02-12 20:13:31
updated: 2018-03-09 23:18:23
tags: ["Linux", "Exploit"]
---

# 绕过 DEP 防护: Return-to-libc

# 使用 Ubuntu cloud image 构建基本测试环境

这时我遇到了一个坑，发现基于 Alpine Linux 测试始终不通过，因此不得不再次改用了 Ubuntu 进行测试。

本来想选择的镜像是 Ubuntu Cloud Image，具体的版本是 Ubuntu xenial 16.04 LTS。Ubuntu Cloud Image 所提供的镜像比较精简，一般大小只有 200MB 多，适合快速下载，同时也提供了 Vmware 的 vmdk 文件下载，比较方便部署，但是具体使用的时候仍然无法支持使用 VirtualBox 启动通过，因此此处仅作一个想法记录下来。

# 调用 System 函数的原理

构造堆栈位调用 system 函数之前所需要的内容，然后使用 jmp 函数模拟对 system 函数的调用

# ROP攻击: 绕过 DEP 和 ASLR 保护

增加了 ASLR 保护后，最明显的改变是不再有固定地址的函数。

# 参考列表

[压栈， 跳转，执行，返回：从汇编看函数调用](https://www.jianshu.com/p/594357dff57e)
[Linux下pwn从入门到放弃](https://xz.aliyun.com/t/1803)

[https://www.jianshu.com/p/ef58e6b0ebef?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation](Linux (x86) Exploit 开发系列教程之八 绕过 ASLR -- 第三部分)

[got、plt表介绍](https://introspelliam.github.io/2017/08/03/pwn/got%E3%80%81plt%E8%A1%A8%E4%BB%8B%E7%BB%8D/)