title: Mac上配置Android开发环境
date: 2017-01-21 19:42:02
tags: ["Mac", "Android"]
---

## 在Mac上配置Android开发环境

在Mac上配置Android开发环境，通常是直接下载安装Android Studio并运行其中的配置，有太多的手工操作，需要多次确认。因此我选择使用Homebrew来安装对应的软件。

Homebrew可以自动安装多个Mac相关软件，以命令行程序为主，而Homebrew Cask包负责安装Mac上的App应用，安装时可以直接搜索，不必去各个网站单独下载，并且可以通过`brew upgrade`进行统一更新，免去了逐一手动更新的痛苦，目前大部分开发相关的应用都可以直接安装，基本能够达到替换App Store。

### 配置基本android开发环境包

正如上文所述，使用Homebrew进行软件管理的目的，所有软件均不会从官网下载，而只通过命令行安装。

```
brew install ant
brew install maven
brew install gradle
brew install android-sdk
brew install android-ndk
```

允许android命令来安装Android开发环境，关于所需的包，可以参考[知乎-Android SDk Manager里面到底哪些东西是必须下载的？](https://www.zhihu.com/question/31935836)

#### 安装HAXM

这是唯一一步需要sudo操作的安装
```
brew cask install intel-haxm
```

### 安装Android Studio

```
brew cask install android-studio
```

允许Android Studio后，记得配置Android SDK路径为/usr/local/opt/android-sdk

###