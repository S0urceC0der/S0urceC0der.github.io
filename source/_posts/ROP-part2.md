title: 从0开始学习Linux ROP利用-part2
date: 2018-02-12 20:13:31
updated: 2018-03-09 23:18:23
tags: ["Linux", "Exploit"]
---

# ROP攻击: 使用 Return-to-libc 绕过 DEP 防护

## 使用 Ubuntu cloud image 构建基本测试环境

这时我遇到了一个坑，发现基于 Alpine Linux 测试始终不通过，因此不得不再次改用了 Ubuntu 进行测试。猜测应该是因为 Alpine Linux 不再基于 glibc，利用方法可能有所改变。

本来想选择的镜像是 Ubuntu Cloud Image，具体的版本是 Ubuntu xenial 16.04 LTS。Ubuntu Cloud Image 所提供的镜像比较精简，一般大小只有 200MB 多，适合快速下载，同时也提供了 Vmware 的 vmdk 文件下载，比较方便部署，但是具体使用的时候仍然无法支持使用 VirtualBox 启动通过，因此此处仅作一个想法记录下来。具体的安装方法不再叙述，同样给虚拟机分配了两个网卡，以方便联网和联入测试。

## DEP 防护原理

前一攻击方法使用的是直接将 Shellcode 写到栈然后执行，通过在编译的时候增加 stack-protector 参数，使得程序栈无法直接执行代码来实现防护。

## 调用 System 函数的原理

伪造堆栈调用 system 函数之前所需要的内容，使得 vulnerable_function 返回时的 jump 变成了实际对 system 函数的调用。这边最理想的情况是调用完 system 函数后，如果 system 函数返回，则能够正常返回到 vulnerable_function 之前的返回地址，程序运行不会出现任何异常。

# ROP攻击: 绕过 DEP 和 ASLR 保护

增加了 ASLR 保护后，最明显的改变是不再有固定地址的函数。所有系统函数的地址均是未知的，不再能够直接调用到这些函数了。但是对应仍然存在一定的绕过方法。其基本思想是找到（泄露）一个已知函数的实际地址，然后通过这个函数和我们需要执行函数的偏移计算出所需要实际调用的函数地址，那么这个问题就转换为如何找到（泄露）一个已知函数的实际地址了。

这里具体使用的方法是依据程序中的 write@plt 函数泄露了

# 参考列表

[压栈， 跳转，执行，返回：从汇编看函数调用](https://www.jianshu.com/p/594357dff57e)
[Linux下pwn从入门到放弃](https://xz.aliyun.com/t/1803)
[Exploit Mitigation Techniques - Data Execution Prevention (DEP)](https://0x00sec.org/t/exploit-mitigation-techniques-data-execution-prevention-dep/4634)

[https://www.jianshu.com/p/ef58e6b0ebef?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation](Linux (x86) Exploit 开发系列教程之八 绕过 ASLR -- 第三部分)

[got、plt表介绍](https://introspelliam.github.io/2017/08/03/pwn/got%E3%80%81plt%E8%A1%A8%E4%BB%8B%E7%BB%8D/)