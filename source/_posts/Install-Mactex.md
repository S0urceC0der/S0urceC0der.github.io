title: 在Mac上配置Mactex
date: 2018-02-25 22:14:39
tags:
---

# Mactex

Mac下使用Latex推荐使用的是Mactex

安装命令

```
brew cask install mactex
```

其中mactex软件包大小达到了

```
brew cask install basictex
```

当然，即使安装了mactex，有时候仍然存在部分依赖的包会不存在，此时需要使用tlgmgr来进行下载软件包

首先更新tlgmgr

```
sudo tlmgr update --self
```

然后更新软件包

```
sudo tlmgr update --all
```

为了加快下载速度，可以配置国内的镜像，例如[清华的镜像](https://mirrors.tuna.tsinghua.edu.cn/help/CTAN/)，但是主要需要将里面的https地址更改为http。

如果出现错误

> ! LaTeX Error: File `nth.sty' not found.
> 
> Type X to quit or <RETURN> to proceed,
> or enter new name. (Default extension: sty)

则可以针对文件名安装对应的包

```
sudo tlmgr install nth
```

# 遇到的问题

tlgmgr 如果使用https的镜像时，会发生错误，错误信息如下，但是

>  tlmgr update --self --repository https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet/
>  /Library/TeX/texbin/tlmgr: open tlpdb(https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet//tlpkg/texlive.tlpdb) failed: Inappropriate ioctl for device at /usr/local/texlive/2017basic/tlpkg/TeXLive/TLPDB.pm line 362.

暂时可以通过将https修改为http的方法绕过

有些软件包名字并不一致，例如nth.sty，通过[搜索](https://tex.stackexchange.com/questions/135402/package-nth-is-in-ctan-but-tlmgr-doesnt-find-it)可以得知包含该文件的包名为gen­misc，可以首先在ctan上搜索，然后找到对应的包名

# 卸载Mactex

Mactex不同版本会安装到不同的目录下，不会覆盖，会占用比较多的空间，有时候需要删除旧版本。可以受限参考[官网的卸载教程](https://www.tug.org/mactex/uninstalling.html)，但是个人删除经验如下

```
sudo rm -rf /Library/TeX/Distributions/TeXLive-2017-Basic.texdist
sudo rm -rf /Library/TeX/Distributions/.FactoryDefaults/TeXLive-2017-Basic
sudo rm -rf /usr/local/texlive/2017basic
```