title: 从0开始学习Linux ROP利用-part2
date: 2018-02-12 20:13:31
updated: 2018-03-09 23:18:23
tags: ["Linux", "Exploit"]
---

# 绕过 DEP 防护: Return-to-libc

# 使用 Ubuntu cloud image 构建基本测试环境

这时我遇到了一个坑，发现基于 Alpine Linux 测试始终不通过，因此改用了 Ubuntu 进行测试。

选择的镜像是 Ubuntu Cloud Image，具体的版本是 Ubuntu xenial 16.04 LTS。Ubuntu Cloud Image 所提供的镜像比较精简，一般大小只有 200MB 多，适合快速下载，同时也提供了 Vmware 的 vmdk 文件下载，比较方便部署，


# 参考

[压栈， 跳转，执行，返回：从汇编看函数调用](https://www.jianshu.com/p/594357dff57e)
[Linux下pwn从入门到放弃](https://xz.aliyun.com/t/1803)


[https://www.jianshu.com/p/ef58e6b0ebef?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation](Linux (x86) Exploit 开发系列教程之八 绕过 ASLR -- 第三部分)