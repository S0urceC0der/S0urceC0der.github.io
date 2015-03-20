title: Node使用心得
date: 2014-12-07 16:49:58
tags:
---

## Node使用心得

#### 更换NPM源

使用[淘宝NPM源](http://npm.taobao.org/)，在使用过程中发现目录`~/.npm/.cache_cnpm/_locks/`是root权限创建的（估计是因为第一次是以root权限运行），在普通权限下使用cnpm时会报错，需要手动修改下。

#### Bower使用

#### Grunt使用

#### bootstrap3-jade-node

基于Grunt基础上，加上了Bootstrap3、Jade、Node模板配置

安装命令：

`npm install -g grunt-express-bootstrap`

初始化库

`grunt-express-bootstrap init`

生成库

```
npm install
bower install
```

本地运行

``


