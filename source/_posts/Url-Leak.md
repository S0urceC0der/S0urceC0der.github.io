title: 网址泄漏
date: 2015-01-03 14:37:26
tags: ["Web"]
---

这个题目起的有点意思。起因很简单，在元旦假期里面突然想折腾一下ArchLinux，装一个Vmware，结果去官网，发现我的Vmware帐号提示处于下载资格审核中，没法直接通过官网下载，而偏偏Vmware Workstation for Linux的下载非常少，或者版本不是最新的（最新的11没有32位版，其他的也普遍是8、9等），后来终于找到一个比较新的[下载地址](http://www.cnblogs.com/zc520/p/3302629.html)（10.0.0），里面提供了10.0.0的官网下载链接：

![Vmware Account Locked](/img/vmware-lock.png)

[http://download3.vmware.com/software/wkst/file/VMware-Workstation-Full-10.0.0-1295980.i386.bundle](http://download3.vmware.com/software/wkst/file/VMware-Workstation-Full-10.0.0-1295980.i386.bundle)

虽然离最新可用32位版10.0.4稍微差点，但仍然是可用的。不过仔细观察这个链接，中间不包含任何随机字符串，因此怀疑可以构造出10.0.4的下载地址。在这个链接中最关键的就是bundle的文件名：VMware-Workstation-Full-10.0.0-1295980.i386.bundle，于是便想直接修改为VMware-Workstation-Full-10.0.4-1295980.i386.bundle，显然这种做法是错误的，因为文件名中含有一个内部版本号1295980，因此必须找到这个内部版本号才能够构造出实际的10.0.4下载地址。

仔细观察[官网下载页面内容](https://my.vmware.com/group/vmware/details?downloadGroup=WKST-1004-LX&productId=362&rPId=7049#errorCheckDiv)，发现真实文件名（内部版本号）在官网上就有，据此构造出实际的10.0.4的下载地址为：

[http://download3.vmware.com/software/wkst/file/VMware-Workstation-Full-10.0.4-2249910](http://download3.vmware.com/software/wkst/file/VMware-Workstation-Full-10.0.4-2249910.i386.bundle)

![Vmware List](/img/vmware-list.png)

## 其他的话

这也不是我第一次干这种事了，基本情况就是下载网址A被大家所知道，但是下载网址B需要通过一定权限才能获取的，然后最关键的是，网址A能够推理出网址B，通常情况下网址里面包含了固定的信息，例如日期，可以直接修改得到。当然，如果有一点随机字符串，就没法获得了。

这个问题相信以前国内的各种下载站都会有过，不过现在应该好多了，但是很多公司自己的软件或者付费下载可能仍然存在这样的问题。如果能够获得一个网址，例如试用版软件或者免费资源，就能够进一步推导出其他资源的网址，当然前提是文件名能够直接从网页上获取到。

## 安装Vmware Workstation中的其他问题

参考了很多[Wiki](https://wiki.archlinux.org/index.php/Vmware)上的内容

#### 无/etc/init.d目录

Archlinux的启动改为systemctl，因此不再有/etc/init.d目录，解决方法是直接忽略错误，然后手工写一个service文件就行了。

#### 编译模块时出错

1. 缺少header.h

查看日志，发现是缺少linux头文件中的header.h，利用locate命令查找到对应的version.h文件，然后创建软链接

`sudo ln /usr/include/linux/version.h /lib/modules/3.17.6-1-ARCH/build/include/linux/version.h`

2. 打补丁

解决了header.h之后，发现还是编译不通过，这下就傻眼了（本事不济搞不定啊！），不过通过搜索后，发现其实Arch Wiki上都写明了，安装软件[vmware-patch](https://aur.archlinux.org/packages/vmware-patch/)就行了，一看这个软件，就已经为10.0.4更新过了，**又让我对Arch产生了进一步的好感，无所不能啊！**

总结就是，有问题，搜**Arch Linux英文Wiki！**
