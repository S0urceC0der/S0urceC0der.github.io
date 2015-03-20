title: Mac
date: 2015-02-15 13:03:43
tags:
---

### 起

人生中第一台Macbook pro，终于在发完年终奖后、回家过年前到手了。

是从苹果团买的，感觉顺丰陆运速度还算快。

装在之前的电脑包里面，就显得电脑包得空间太大，需要买个内胆包才行。

### 用

首先要学习的是触摸板手势，有了这个真心不想用鼠标了，总算能够完全使用键盘（且算触摸板为键盘的一部分吧）

键盘使用起来按键声音还好，比较清脆舒服，虽然键程不长，但是感觉还行吧。的确有必要换个外置键盘。

字体看的真是舒服，终于不用在Win/Linux下面费尽心思的调整字体了。

### 软件
一大堆软件都不会用。

#### 常用软件

登陆AppStore，进免费排行榜，下载了QQ、搜狗输入**板**，其中搜狗输入**板**的功能是通过里面的入口安装搜狗输入法。
安装Xcode，虽然目前一次都没有打开过。
首先安装Homebrew，幸好最近在折腾Linuxbrew，很多操作都是一样的

对于使用Homebrew安装的系统软件，如数据库等，根据提示，相关启动方法如下

`
To have launchd start mongodb at login:
    ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
    ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
Then to load mongodb now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
Or, if you don’t want/need launchctl, you can just run:
    mongod --config /usr/local/etc/mongod.conf
    redis-server /usr/local/etc/redis.conf
`

#### zsh

具体参考Mactalk的这篇文章[终极shell](http://macshuo.com/?p=676)

另外，在使用zsh的agnoster主题时，需要额外下载增强的字体，并在终端选择使用。
