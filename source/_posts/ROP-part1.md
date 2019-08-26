title: 从0开始学习Linux ROP利用-part1
date: 2017-11-22 21:19:23
updated: 2018-01-19 22:58:03
tags: ["Linux", "Exploit"]
---

# 基础

ROP(Return-oriented programming) 是一种通过覆盖堆栈 (Stack) 内容来实现控制返回地址实现执行自定义代码的漏洞利用手段。目前比较流行的教程是蒸米写的Linux ROP系列，本文以此为基础，综合其他文章来详细的讲解Linux ROP利用国产。

# 构造虚拟机环境

测试过程中需要x86/x64两个环境的Linux系统，因此一般物理机测试起来比较麻烦，可以通过安装虚拟机来构造对应的测试环境。这里我使用 VirtualBox 来安装不同的测试环境，此外我选择基于 Alpine Linux 搭建测试环境，原因是 Alpine Linux 的镜像相对比较小，而且里面是基于 musl libc 实现了系统调用，意味着网上教程中的很多地方需要自己进行针对性的修改，这也是一个挑战。

## 虚拟机网络配置

为了方便测试，我选择使用 SSH 连接 Linux 主机执行命令，而不是从 VirtualBox 的界面，无法快捷的复制粘贴内外内容。使用 SSH 时需要有一个条件是物理机主动连接到虚拟机，但是默认的虚拟机环境采用 NAT 模式，虚拟机能够访问外网但是物理机无法访问虚拟机，这时可以额外添加一个 Host Only 网络来连接虚拟机。

## Alpine Linux 基础环境搭建

需要手动安装 gcc 和 libc-dev 库提供基础编译环境，安装 gdb 提供调试功能。

```
apk add libc-dev gcc gdb
```

# 静态分析代码

## 反汇编程序

为了方便反汇编，可以将 gdb 的语法设置默认为 AT&T 语法改为 Intel 语法，具体方法如下：

```
echo "set disassembly-flavor intel" > ~/.gdbinit
```

## GDB 指令简介


* `b[reak] <行号|函数名|代码地址>` 下断点

* `r[un]` 运行代码

* `c[continue]` 继续运行

* `s/n` Step into 单步跟踪进入/Step Over 单步跟踪

* `si/ni` 汇编指令集 `s/n` 

* `p [varialbe/register]` 打印变量/寄存器


## Debug 教程

使用 `gdb level1` 来加载程序，然后调用 `disas vulnerable_function` 来反汇编存在漏洞的函数。输入 `layout asm` 分屏显示汇编代码， `b  vulnerable_function` 下断点，然后 `r` 开始运行程序，`p $eip` 打印当前 EIP 寄存器地址。

```
   0x00001220 <+0>:	    push   ebp
   0x00001221 <+1>:	    mov    ebp,esp
   0x00001223 <+3>:	    push   ebx
   0x00001224 <+4>:	    sub    esp,0x84

   0x0000122a <+10>:	call   0x1218 <__x86.get_pc_thunk.ax>
   0x0000122f <+15>:	add    eax,0x2d99
   0x00001234 <+20>:	sub    esp,0x4          // <----a

   0x00001237 <+23>:	push   0x100             // 256
   0x0000123c <+28>:	lea    edx,[ebp-0x88]    // -+
   0x00001242 <+34>:	push   edx               // -+--  ebp-0x88为buf地址
   0x00001243 <+35>:	push   0x0               // STDIN_FILENO
   0x00001245 <+37>:	mov    ebx,eax
   0x00001247 <+39>:	call   0x1040 <read@plt> // 调用read函数

   0x0000124c <+44>:	add    esp,0x10
   0x0000124f <+47>:	nop
   0x00001250 <+48>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x00001253 <+51>:	leave                    // mov esp,ebp; pop ebp;
   0x00001254 <+52>:	ret                      // pop eip; jump eip;

   -----------------------

   0x0000126f <+26>:	call   0x1220 <vulnerable_function> // push eip; jump XX;

```

汇编代码中有一个函数 `__x86.get_pc_thunk.ax` 的调用，在此可以暂时忽略这段代码，与 EOP 利用无关，没有任何影响。关于这个函数的详细解释，参考 [《什么是__i686.get_pc_thunk.bx？我们为什么需要调用这个？》](https://cloud.tencent.com/developer/ask/91473) 一文。

## 堆栈分析

下面先从理论上对堆栈情况分析一下。首先要注意的是，esp 指针是指向栈顶，即当前已经使用的空间。执行汇编指令到 a 处，堆栈如下：

![初始堆栈](/img/rop_stack_1.png)

通过`si`和`ni`指令执行到read函数之前，此时的堆栈如下图2：

![调用read函数前的堆栈](/img/rop_stack_2.png)

通过调试发现此时堆栈中有两个空位 0xbffffcb0 和 0xbffffc2c，猜测可能是多余的代码导致的。调用完 read 函数，输入内容后，进行堆栈平衡，此时堆栈变成下图：

![调用read函数后的堆栈](/img/rop_stack_3.png)

调用指令 `x /8cb 0xbffffc30`  可以显示出输入的前 8 个字符。继续往下执行，这时候会返回，会调用 `leave` 指令再次堆栈平衡，释放函数中的临时变量，并恢复原有函数栈帧的 esp 。最后执行 `ret` 指令，至此整个执行流完毕。

## 漏洞原理

很明显，read 函数没有对输入的内容校验长度，导致可以输入超长字符串，利用字符串覆盖 eip(0xbffffcbc) ，则我们就可以控制这个函数的返回地址，实现漏洞利用。通过计算可以得到 0xbffffcbc - 0xbffffc30 = 0x8c，即使用 0x8c + 0x04 长度的字符串覆盖，最后的四字节就可以覆盖成功。对比原文中提供的通过脚本获取覆盖长度脚本，可以发现与我们分析的一致。

## 生成 Shellcode

很可惜，使用教程中所提供的 shellcode 时存在问题，进程仍然 crash，不过使用 Metasploit 生成要的 Shellcode 却能够正常执行。生成 Shellcode 的具体做法如下，进入 msfconsole，使用 `show payloads` 可以看到所有支持的 payload。然后 `use linux/x86/exec` 来选用当前环境的 payload ，输入 `info` 查看当前 payload 的参数，用 `set CMD /bin/s` 来生成执行 `sh` 的 shellcode，最后使用 `generate` 命令来生成。

## 执行 Shellcode

最初的想法是在 Alpine 中安装 pwntools 工具本地执行，但是发现安装了一系列软件后，最终因为某个库不支持 x86 环境而安装失败，相关需要安装的库如下，有需要的可以参考下：

```
apk add --no-cache -X https://mirrors.tuna.tsinghua.edu.cn/alpine/edge/community/ capstone-dev
apk add python3 python3-dev alpine-sdk libffi-dev openssl-dev
pip install pwntools
```

因为 pwntools 安装失败，所以使用 socat 命令来起服务，`socat TCP4-LISTEN:5001,fork EXEC:./level1`，这里同样要重新寻找对应的返回地址，因此第一次可以输入一个错误的 Shellcode，然后同样使用`gdb level1 core.xxxxx` 命令来调试 coredump 文件，显示具体的返回地址，再重新修改 shellcode 中的返回内容。

![执行成功](/img/rop_execute.png)

# 后记

虽然这个教程篇幅不长，但是自己复现整个过程却充满了坎坷，断断续续花了两个月的时间，所幸第一次总是艰难的，后续会应该轻松很多吧，加油！

# 参考文章列表

[VirtualBox虚拟机和Mac或Win主机之间网络相互通信](https://segmentfault.com/a/1190000012756506)

[一步一步学ROP](https://github.com/zhengmin1989/MyArticles/tree/master/%E4%B8%80%E6%AD%A5%E4%B8%80%E6%AD%A5%E5%AD%A6ROP)

[一步一步学ROP 镜像文 ](http://drops.2xss.cc/?chamd5#!/drops/595.%E4%B8%80%E6%AD%A5%E4%B8%80%E6%AD%A5%E5%AD%A6ROP%E4%B9%8Blinux_x86%E7%AF%87)

[Introduction to Return Oriented Programming (ROP)](https://ketansingh.net/Introduction-to-Return-Oriented-Programming-ROP/)

[栈溢出漏洞的利用和缓解](https://www.cnblogs.com/pannengzhi/p/exploit-the-stack.html)

[StackOverFlow之Ret2ShellCode详解](https://www.freebuf.com/vuls/179724.html)

[常用GDB指令](https://www.jianshu.com/p/b7896e9afeb7)