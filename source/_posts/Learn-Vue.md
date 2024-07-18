title: "Vue入门：搭建在线视频播放"
date: 2020-09-06 14:39:48
tags:
---

## 起因

一些常看的视频 APP 或者网站广告太多，逐一写广告屏蔽规则过于麻烦，但是播放地址却很好发现，能够直接爬取下来，因此想着直接做一个简单的视频播放网页，顺带学习一下 Vue。

## 搭建基础开发环境

```
brew install node        // Mac 下使用 Homebrew 安装 node
npm install -g crm       // 安装源管理工具，加速下载
npm install -g @vue/cli  // 安装 vue-cli
vue create video-sea     // 创建项目
cd video-sea
npm run serve            // 启动服务
```

## 内容开发

为了减少开发工作量，使用 ElementUI

```
vue add element         // 添加 Element UI 
vue add route           // 添加 vue route
```

因为我选择的是按需加载模式，所以在使用每个组件前需要手动加载

```
import { Button, Radio } from 'element-ui'

Vue.use(Button)
Vue.use(Radio)
```

注意：已经不需要写分号了。