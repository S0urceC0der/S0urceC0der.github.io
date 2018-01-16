
title:  破解某网站m3u8视频资源
date: 2017-10-02 10:37:29
tags: ["Flash", "Web"]
---

---

# 破解某网站m3u8视频资源

## 背景

某个视频教学网站资源即将超过有效期，因此尝试将网页上的视频下载到本地。

## 过程

### 1. 登录验证

这部分非常简单，模拟提交登录表单，并及时保存对应的Cookie即可

### 2. 视频播放流程

这个网页上使用了一个Flash播放器，然后通过该Flash文件读取m3u8文件进行播放。

HTTP Live Streaming（HLS）是苹果公司(Apple Inc.)实现的基于HTTP的流媒体传输协议，m3u8，是HTTP Live Streaming直播的索引文件。该文件包含了这个视频的一系列视频分片，Flash播放器或者


```
    #EXTM3U
    #EXT-X-VERSION:3
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-ALLOW-CACHE:YES
    #EXT-X-TARGETDURATION:11
    #EXT-X-KEY:METHOD=AES-128,URI="http://ztest.qiniudn.com/crypt0.key",IV=0xe532855feb3e18366b8e7ea0c11f3116
    #EXTINF:10.066667,
    http://ztest.qiniudn.com/Fr88-3sZu8HqPFot_BapyYtuz3k=/FgCBc3IlydY6CFIA8jhe7jIxCt1y/seg0
    #EXT-X-KEY:METHOD=AES-128,URI="http://ztest.qiniudn.com/crypt0.key",IV=0x48586a2ac8397fbce9565480259c1b94
    #EXTINF:10.000000,
    http://ztest.qiniudn.com/Fr88-3sZu8HqPFot_BapyYtuz3k=/FgCBc3IlydY6CFIA8jhe7jIxCt1y/seg1
    #EXT-X-KEY:METHOD=AES-128,URI="http://ztest.qiniudn.com/crypt0.key",IV=0x928f18982f6ee1a7e36cfa8f36979c3a
    #EXTINF:10.000000,
    http://ztest.qiniudn.com/Fr88-3sZu8HqPFot_BapyYtuz3k=/FgCBc3IlydY6CFIA8jhe7jIxCt1y/seg2
    #EXT-X-KEY:METHOD=AES-128,URI="http://ztest.qiniudn.com/crypt0.key",IV=0x6651941d56de8af0c7d4bee9ae33a8de
    #EXTINF:10.000000,
    http://ztest.qiniudn.com/Fr88-3sZu8HqPFot_BapyYtuz3k=/FgCBc3IlydY6CFIA8jhe7jIxCt1y/seg3
    #EXT-X-KEY:METHOD=AES-128,URI="http://ztest.qiniudn.com/crypt0.key",IV=0x90df003d61ba2ef9413fdaf521cfce15
    #EXTINF:10.000000,
    http://ztest.qiniudn.com/Fr88-3sZu8HqPFot_BapyYtuz3k=/FgCBc3IlydY6CFIA8jhe7jIxCt1y/seg4
    #EXT-X-KEY:METHOD=AES-128,URI="http://ztest.qiniudn.com/crypt0.key",IV=0xc7773183806b8d3d7e44811076ed5b66
    #EXTINF:2.200000,
    http://ztest.qiniudn.com/Fr88-3sZu8HqPFot_BapyYtuz3k=/FgCBc3IlydY6CFIA8jhe7jIxCt1y/seg5
    #EXT-X-ENDLIST
```

m3u8内容如上所示，解析时一般是根据`EXT-X-KEY`中的`METHOD`的加密方法和`URI`链接下载密钥内容，最终结合`IV`解密后续的分片url。

### 3. 变种

上述是正常的播放过程，但是对该网站解析时却失败了，解密出来的结果却不是正常的视频，多次尝试后，唯一的剩下的可能性便是这个Flash播放器采用了变种HLS协议。

使用[ffdec](https://www.free-decompiler.com/flash/download/)打开下载得到的Flash播放文件，阅读其中的代码，果然并非标准的HLS协议。
