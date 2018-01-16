title: Firefox57新版试用
date: 2017-11-26 17:18:30
tags: ["Web"]
---

# 浏览器使用历史

## 早期试用

记得最早使用的浏览器是傲游，后来短暂使用了一段时间的搜狗之后就一直改用Firefox，即使Chrome出来后，短暂使用过，但是后面还是继续选择使用Firefox。

Firefox给我的第一印象就是丰富的插件，相比早期的IE和国内一众基于IE内核的浏览器，插件功能更加强大，特别是那会惊为天人的Adblock Plus让我摆脱了页面上各种眼花缭乱的广告。而另外一个插件Downthem All则让我在大多数情况下不需要启动迅雷来多线程下载一个本来就不会花费太多下载时间的文件链接，尤其。

## Chrome

虽然后来出现了速度更加快的Chrome，但是一开始的某些插件（例如Adblock）并不完善，并且处于对隐私方面的考虑，仍然坚持使用Firefox。

Chrome也在不停的进步，各种功能不断完善，所以目前我主要会在调试网页的时候试用Chrome。

## Firefox缺点

目前使用的感觉主要是实在是太慢了，标签一多经常性的会出现卡死。在Windows下仍然是32位，所以都会单独下载第三方编译版本（例如pcxfirefox），而在Linux/Mac下使用时，则要经常性的重启下。

为了应对Chrome的竞争，近两年来Firefox的采取了更加激进的更新策略，但是随之而来的却是每个版本一旦更新，一些插件就会失效，特别是Autoproxy这个插件，一度让我想放弃Firefox。而近期，则完全抛弃了原有的插件支持，引入了基于WebExtension的插件，增加了插件的通用性，提高了安全性。

# Firefox 57试用体验

第一次装上Firefox57后，的确觉得其速度变快很多，但是仔细观察下来，更多的感觉快是显示在了页面加载到完成渲染的过程中，

## 新版附加组件列表

因为大量的插件不再可用，所有需要重新选择浏览器插件，这是我在Firefox 57上所使用的新版插件列表，基本能够满足我的日常需求。

### Evernote Web Clipper

### Greasemonkey

虽然有人推荐试用Tampermonkey替代，但是目前为止Greasemonkey对于我仍然能够满足需求。

### HTTPS Everywhere

自动跳转到该网站的HTTPS版本，随着HTTPS的不断普及，相信这个插件会逐渐失去其功能。

### LastPass

密码管理，类似的还有其他几款管理软件。

### Proxy SwitchyMmega

以前的代理插件AutoProxy及后续的第三方更改版本基本每次Firefox一升级就要死一大片然后重新找，现在这个插件是Chrome上的对应移植版本，不用再担心一升级就失效了。目前试用国产中仍然感觉有一些bug。

### uBlock Origin

新一代的广告过滤插件，对于Adblock Plus有着较大的优势。目前Adblock Plus已经堕落了，在社区内引起了几次争论，使得大家都对其持怀疑态度。

### uMatrix

用于屏蔽网页无关脚本，可以辅助屏蔽第三方追踪脚本，是NoScript的替代品。

### Vimium

Vim快捷键插件，但是比原来的Vimperator权限小了很多，页面加载完成后才有效果。

### Open Tabs Next to Current

新版插件功能不再有对标签外观和标签位置的更改能力，为此这个插件好像使用了一种方法绕过了限制，能够紧接当前标签新建一个新标签，此外在设置里面可以针对从当前页面打开的链接的标签位置进行配置。

>   about:config 设置browser.tabs.insertRelatedAfterCurrent 为true

## 缺少的插件

### Down Them All

不再有类似的多线程轻量级下载工具，需要自己额外配置第三方下载软件。

## Firefox57不足

目前的问题主要如下：

1. Firefox57的SwitchyOmega会经常性的出现菜单显示不正常，参考[https://github.com/FelisCatus/SwitchyOmega/issues/1315]

2. 在Mac下，Firefox57经常性的会出现高CPU占用情况，然后风扇狂转，最终我更换成了Firefox Nightly。