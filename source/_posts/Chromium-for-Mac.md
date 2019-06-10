title: Mac 上安装 Chromium
date: 2019-06-08 16:35:13
updated: 2019-06-10 23:04:26
tags:
---

最开始我一直是 Firefox 党，也简单写过了几篇关于 Firefox 使用的文章，例如[《Firefox57新版试用》](/2017/11/26/Firefox57/)，但是后来发现 Firefox 在 Mac 上经常性的出现风扇狂转的现象，因此不得不转移到了 Chrome 上，所幸两者的丰富插件使得使用体验上基本一致。但是近几年 Chrome 的隐私问题越来越严重，我也不得不对持续使用 Chrome 打上了问号。因此想着能否使用 Chromium 来替代。

# Chromium 介绍

众所周知，Chrome 是谷歌开发的一个网页浏览器，但是这个浏览器也不是完全从零开始的，而是采用了大量的开源代码，最重要的部分就是苹果的 Webkit，谷歌也开源了大量 Chrome 相关的源码回馈到开源社区，著名的 NodeJS 就是基于谷歌开源的 V8 Javascript 解释引擎实现的的。基于谷歌的 Chrome 开源代码内容，编译得到的就是 Chromium。

相比 Chrome，Chromium 缺少了某些闭源的二进制代码，比如 Flash 插件，商业解码器等，也不会有任何谷歌的标识，因此国内的浏览器厂商也都选择基于 Chromium 来作为他们自己浏览器的内核。

正如上文所说，Chromium 中缺乏了部分闭源代码，对此主要是有两方面的影响，第一是很多 Chrome 自带的服务都无法使用，这个恰恰是我们所希望的，缺乏谷歌的服务能够大幅提升隐私性；第二个影响则是缺乏视频解码，这个则会对我们平时的网页浏览带来非常大的影响。因此默认 Chromium 官网提供的并不能实际使用，而我选择的是开发者 marmaduke 编译的带有解码器的版本。关于 Chromium 可用版本的列表可以参考网页 https://chromium.woolyss.com/，该页面提供了致命的第三方编译 Chromium 列表，可以自行选用。

最终，我选择了自带解码器的 marmaduke 编译的 Chromium，值得注意的是，该作者提供了三个版本的 Chromium 分别是。

* marmaduke-chromium 最基础的版本，猜测应该是标准 Chromium 外加视频解码器
* marmaduke-chromium-nosync 去除了 Chromium 中的 Google Sync 和 Widevine（一种加密视频协议）。
* marmaduke-chromium-ungoogled 更为激进的一个版本，去除了谷歌的同步，各种服务，以及相关的二进制文件，同时也带来了一些安全上的改进，具体的内容可以参考 [UNGOOGLED Chromium](https://github.com/Eloston/ungoogled-chromium/blob/master/README.md)，注意这个版本无法从 Chrome Webstore 下载和更新扩展程序。 

唯一要注意的是上述三个不可以同时安装，简单挑选一个即可，在 Mac 上的具体安装步骤如下：

```
brew tap cpbotha/marmaduke-chromium
brew cask install marmaduke-chromium
brew cask install marmaduke-chromium-nosync
brew cask install marmaduke-chromium-ungoogled
```

至于我的个人选择，本来是想优先选用 marmaduke-chromium-ungoogled，但是因为扩展程序更新不方便，还是退而求其次选择 marmaduke-chromium 吧。

# 扩展持续列表

最后，仍然仿照 Firefox 列出我所使用的扩展列表（按照字母排序）：

## 1. cVim

可以仿照 Vim 的快捷键来进行网页浏览。

## 2. HTTPS Everywhere

自动访问加密版本的网址。

## 3. LastPass

密码管理。

## 4. Proxy SwitchyOmega

自动翻墙代理。

## 5. Save to Pocket

保存网页到 Pocket。

## 6. Tampermonkey

油猴子脚本插件。

## 7. uBlock Origin

屏蔽广告。

## 8. uMatrix

屏蔽网页内容和脚本。
