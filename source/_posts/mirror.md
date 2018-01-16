title: 常见软件包的国内镜像
date: 2016-01-10 23:45:19
tags: ["Linux"]
---


在Linux/Mac上经常要使用各种软件库来下载资源，但是这些资源链接经常被墙，或者速度较慢，因此需要改用国内的镜像站点来加快速度。

### 常见开源镜像站点

这些站点包含常见各种的Linux软件源。

[中国科技大学开源镜像](https://mirrors.ustc.edu.cn/)
[清华大学开源镜像](http://mirrors.tuna.tsinghua.edu.cn/)
[阿里云开源镜像](http://mirrors.aliyun.com/)

其中中国科技大学使用的是https，因此在大多数情况下推荐使用（2016年底科大源曾长期处于不稳定状态，目前已经恢复），阿里云镜像适合阿里云的服务器使用，可以免流量且速度快。

### 具体一些推荐配置

#### python

网上大多推荐豆瓣的源，但是豆瓣的源仍然是http协议，新版pip会提示警告，所以还是换成科大源吧（除非是低版本Linux不支持https链接）。此外，使用阿里源还遇到过几次同步不及时的问题。

#### node

推荐使用阿里源，主要是提供额外的cnpm命令，可以方便的安装。

#### Ruby

淘宝的Ruby镜像已经改由Ruby-China维护，所以请使用[Ruby-China的镜像](https://gems.ruby-china.org/)

```
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
$ gem sources -l
https://gems.ruby-china.org
# 确保只有 gems.ruby-china.org
```

#### Android

使用腾讯Bugly镜像，[将Android SDK Manager的代理设置为android-mirror.bugly.qq.com:8080](http://android-mirror.bugly.qq.com:8080/include/usage.html)

#### homebrew

homebrew自身内容是放在github上的，如果更新速度慢，可以换成[清华的镜像](http://mirrors.tuna.tsinghua.edu.cn/help/#homebrew)：

```
$ cd /usr/local
$ git remote set-url origin git://mirrors.tuna.tsinghua.edu.cn/homebrew.git
```

homebrew-science或者homebrew-python也可以参考其页面进行修改。不过如果不是处于极其艰难的情况，不建议改这个，因为无法确保各个的软件包的hash未被篡改。

homebrew大多数的软件源都是在homebrew.bintray.com上的，速度同样非常慢，[有人](http://ban.ninja/)也做了份国内镜像。

设置环境变量:

```
export HOMEBREW_BOTTLE_DOMAIN=http://7xkcej.dl1.z0.glb.clouddn.com
```

#### Docker Hub

国内基本斗士要注册的，科大源有个反向代理，因为Docker配置比较麻烦，请具体参考[Docker镜像使用帮助](https://lug.ustc.edu.cn/wiki/mirrors/help/docker)

#### pyenv

同样设置环境变量
```
export PYTHON_BUILD_MIRROR_URL="http://pyenv.qiniudn.com/pythons/"
```

#### 注意事项

对于这些第三方源，并不能完全保重其安全性，所以会存在一定的风险下载到经过篡改的软件包。

### 更新

2017.01.21 更新多条源记录