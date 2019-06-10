title: ROP 学习之 Shellcode
date: 2018-01-23 21:42:52
updated: 2018-01-27 17:32:12
tags: ["Linux", "Exploit"]
---

# 起

上一篇学习 ROP 的文章中提到原文的 Shellcode 调用失败了，这次就尝试分析下该 Shellcode 看看能不能找出原因。

# Shellcode 反汇编

拿到一段 Shellcode，第一步就是尝试反汇编，参考文章《使用 Python 将 Shellcode 转换成汇编》文章教程，基于 capstone 使用如下代码即可。

```
#!/usr/bin/env python2
# -*- coding:utf-8 -*-

from capstone import *

for code in shellcode:
    md = Cs(CS_ARCH_X86, CS_MODE_32)
    for i in md.disasm(code, 0x00):
        print("0x%x:\t%s\t%s" %(i.address, i.mnemonic, i.op_str))
```

从这段源码可以看到，这是利用 capstone 的反汇编功能来实现的，但是需要指定对应汇编的处理器架构和字长信息。

# 分析 Shellcode

# 参考链接

[【Python】使用Python将Shellcode转换成汇编](https://bbs.pediy.com/thread-222965.htm)

[Linux下shellcode的编写](https://xz.aliyun.com/t/2052)