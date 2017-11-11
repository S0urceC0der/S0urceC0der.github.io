
title:  在无Root权限系统中安装Linuxbrew
date: 2016-01-12 02:37:25
tags:
---

---

# 在无Root权限系统中安装Linuxbrew

## Linuxbrew

Homebrew是Mac系统下著名的软件安装管理系统，其特点是不需要root权限即可编译安装Linux下的各种软件，通过修改路径来优先使用安装的软件包。而Linuxbrew则是其Linux移植版本，同样可以在不需要root权限的情况下，在.linuxbrew下完整建立一个运行环境，并在环境变量里面优先使用。

因为目前我使用的一个系统是Centos5.4版本，无root权限，gcc、python等版本均非常旧，而且无法自由安装软件提供工作效率，所以选择Linuxbrew来安装需要的软件。但是实际上整个安装过程也是非常艰难的。

## 安装过程

虽然Linuxbrew号称不需要root权限，但是实际安装的时候还是需要满足一定的条件，目前主要条件如下：

>   * Ruby 1.8.6 or newer
>   * GCC 4.2 or newer
>   * Linux 2.6.16 or newer
>   * 64-bit x86 or 32-bit ARM platform

可惜现有的系统不满足前两个条件，意味着需要在没有root权限下实现这亮点。

### Ruby依赖

Linuxbrew是基于ruby的，而Centos5.7的ruby版本是1.8.5，仅仅相差了一个小版本号，却无法运行。因为没有root权限，所以选择采用[rvm](https://rvm.io/)升级安装ruby。

默认rvm是编译安装的，但是显然当前版本没有安装必要的devel包，所以是无法编译成功的，看到安装过程中会首先去搜索precompiled binary，猜测可能有不需要编译的ruby版本提供，经过一番搜索，果然是可以的，运行`rvm list remote`可以看到支持当前系统的ruby预编译版本，就选择了一个最低版本ruby-1.9.3-p551，但是很不幸，应该是网络问题无法直接下载，需要手动从[下载链接](https://rvm_io.global.ssl.fastly.net/binaries/centos/5/x86_64/)进行下载。

对于下载的包，运行`rvm mount -r ruby-1.9.3-p551.tar.bz2 --binary`加载，会自动把包内的文件释放出来，此时会提醒运行缺少libyaml，但是发现好像并不会引起其他问题。

### Git依赖

坑爹的是git也没有安装，第一想法当然是直接释放git的rpm包，但是运行的时候一直有错误提醒：

>    $ git clone https://github.com/nvie/gitflow.git
>    Cloning into gitflow...
>    fatal: Unable to find remote helper for 'https'

后得知是缺乏libcurl相关库，但是最初并不知道，因此选择重新编译git，官网提示需要如下库

```
sudo yum install curl-devel expat-devel gettext-devel openssl-devel perl-devel zlib-devel
```

自然是按照对应的依赖，然后下载解压对应的包，增加到include路径中，然后进行编译，但是事实证明这个过程非常艰难，最终没有编译成功，有兴趣的同学可以尝试下看看。

幸运的从网上找到了一个[Centos rpm包库](http://vault.centos.org/5.4/os/x86_64/CentOS/)，然后根据需要的包名选择下载，s使用命令`rpm2cpio *.rpm | cpio -div`释放rpm包文件到了本地的~/usr目录下，并确认这些文件及so能够正常在环境变量里面

```
export PATH=$PATH:~/usr/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/usr/lib
export C_INCLUDE_PATH=$C_INCLUDE_PATH:~/usr/include
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:~/usr/include
```

随后编译git，并放入~/usr/bin下，能够正常运行git。

### Standalone模式安装Linuxbrew

在Standalone模式下，Linuxbrew会搭建一个完全脱离现有系统的编译环境和运行环境，运行Linuxbrew的软件时不再依赖系统原有的的编译器和库。

虽然说是Standalone模式，仍然需要一个编译器来首先编译出一个gcc，但是CentOS5.4的默认gcc版本过旧，必须要求最低4.4版本才行。依然尝试下载对应的rpm包，然后需要注意的是，这些包可能存在依赖，所以需要运行命令`rpm -Uvh --test *.rpm`来查看是否已经安装，否则仍然需要手动安装。我正是因为libstdc++44-devel-4.4.0没有安装导致耽误了两天的时间。具体的包如下：

>    libstdc++44-devel-4.4.0 gcc44-4.4.0 gcc44-gfortran-4.4.0 gcc44-c++-4.4.0

安装glibc的时候，官方指南有一句提示：

>    before this, you may want to `brew edit glibc` to produce compatibility for your particular kernel, for example:
>    "--enable-version=2.6.18"

我并未搞懂`--enable-version`参数的作用，只是在查找glibc编译选项时，看到需要开启对应内核的支持`--enable-kernel=`

剩下的安装过程可以参考[官方指南](https://github.com/Homebrew/linuxbrew/wiki/Standalone-Installation)，大体流程就是先编译出gcc，再编译glibc，然后重新编译依赖这个glibc的gcc，完成整个编译工具链，最终安装基础的软件包，安装ruby、git替换原有的库。

此外安装Git的依赖选项homebrew/dupes/tcl-tk过程中还有有一个小意外，参考(https://github.com/Homebrew/linuxbrew/issues/369) ，`brew install tcl-tk --without-tk`

最后ruby安装的时候报错，就先凑合着用rvm吧，可能后面再过一阵子fixbug就好了

## 其他小问题

1. 旧版git会遇到各种HTTPS的问题，例如提示git 证书问题

git config --global http.sslVerify false

2. 安装时候提示tmp目录问题

>   Error: parent directory is world writable, FileUtils#remove_entry_secure does not work; >    abort: "/tmp/homebrew20160101-27874-8ry2dn" (parent directory mode 40777)

参考链接 (https://github.com/Homebrew/homebrew/issues/39475) 设置`export HOMEBREW_TEMP=~/.tmp/`，或者修改文件`~/.linuxbrew/Library/Homebrew/config.rb`内的temp内容

3. 下载问题

下载不稳定，所以经常无法下载，可以手动下载然后把文件放到`~/.cache/Homebrew`下，如果没有正常识别出缓存的话，可以本地搭个简单的http服务器（`python -m SimpleHTTPServer`)，然后`brew edit package`去修改下载链接

### 使用zsh

安装zsh后，自动安装oh-my-zsh会出现检测不到zsh的问题（当然是因为不在默认的shell路径中了），需要手动下载这个安装脚本，并且设置其中的`CHECK_ZSH_INSTALLED`为1。因为没有权限切换默认的shell，所以基本是不能用zsh的，不过有个曲线救国的方法，就是安装tmux并且让tmux启动时自动调用zsh，以后每次启动tmux就启动调用zsh。设置方法为在文件`.tmux.conf`中写入：

```
set-option -g default-shell ~/.linuxbrew/bin/zsh
```

## 后续更新记录

### Linuxbrew更新情况

2016年四月Homebrew和Linuxbrew都进行了目录结构的更改，将原有的包分割成了brew和Homebrew-core两个包，需要进行重新调整。

### 新的安装方法

最新的Homebrew已经不再需要上述如此复杂的方法了，可以参考最新的安装方法(https://github.com/Linuxbrew/brew/wiki/CentOS6) ，通过`HOMEBREW_NO_AUTO_UPDATE=1 HOMEBREW_BUILD_FROM_SOURCE=1 brew install gcc --without-glibc` 进行安装编译，但是其他的依赖安装仍然需要同样的方法进行设置。

### 不在支持CentOS5旧版本

最新的glibc-2.23要求linux内核至少为2.6.16，因此最新的Linuxbrew已经无法安装再CentOS5的旧版本。如果硬要安装，个人感觉可以搜索homebrew-core的glibc为2.19历史版本覆盖最新的homebrew-core来进行安装，但是我因为gmp编译问题，并未安装成功。

## 总结

为了能够在自由安装软件也是醉了，折腾了好久，不过安装完毕后，也是非常舒服的。
