title: 配置安装 Onedrive 相关工具
date: 2019-09-18 11:57:09
tags:
---

# 简介

Onedrive 是微软提供的一个共享网盘服务。随着 360 云盘的下线和百度云盘的下载限速，国内好用的网盘越来越少。之前也找过不少的软件实现百度云盘的高速下载，但是最近已经被屏蔽的差不多了，唯一的方法是购买 SVIP 帐号了，评估之下觉得还是需要将自己搜集的大部分公用数据分散转移，因此选择了 Onedrive 进行存储。

最初的 Onedrive 国内下载速度较慢，但是目前经过网络优化，下载速度提高了很多，大文件的下载速度能够接受，甚至看到有些人直接利用 Onedrive 做视频点播。最主要的一点是很多人基于 Office 的开放 API 实现了很多基于 Onedrive 的程序，大大提高了可用性，使得我这样不经常使用 Windows 的人也能够受益其中。

# 申请帐号相关

注册 Onedrive 的，按照安全与可持久性的原则列出如下：

### 1. 个人/家庭订阅 Office 365

最可靠的渠道，个人版 398元/年一个用户，家庭版 498元/年六个用户，有一定的经济成本。

### 2. Office 365 教育版

使用与微软合作的大学的edu邮箱可以注册。这个限制在于并不是所有的学校都在范围内，因此需要你查看学校的具体情况。此外根据微软的条款，虽然毕业后可以继续使用，但是微软官方的说明是*我们可能会随时验证您是否符合条件*。注意可以选择注册为教职员工来规避这一点，此外学校管理员可以看到个人存储的数据。

### 3. Office 365 E5 开发人员订阅

针对开发人员的优惠订阅，缺点是*你的订阅可使用 90 天，并且基于有效开发人员活动续订。 如果正在将订阅用于开发，订阅将每 3 个月续订一次，可以无限期延期*。而且网上目前也没有*有效开发人员活动*的具体定义，因此存在被突然停用的风险。

### 4. 第三方买家购买

价格不会很高，但是这种帐号的来源乱七八糟，需要自己辨别。最重要的这种帐号的管理员肯定能够浏览你个人存储的数据，有一定的数据泄露风险。

# OnedriveCMD

OneDriveCMD 是一个 Onedrive 的命令行版本，可以通过命令行进行文件的上传和下载。

推荐安装 Python 3.x 版本的 OnedriveCMD，能够有效的处理中文字符串，避免一些问题。

如果存在多个账户，可以利用 `onedrive -conf config.json` 来指定所使用的配置文件，实现多个账户。

如果觉得命令行使用局限性太多，可以使用 rclone 将 Onedrive 挂载到本地目录上，深度整合如操作系统。

# OlaIndex

OlaIndex 是一个提供 Onedrive 在线文件目录访问的服务，能够直接下载上传文件，并在线观看视频。

之前网络上最常用教程的是使用 Now.sh 在线搭建 OneIndex，但是随着 Now.sh 逐渐下线对 Docker 容器的支持，架构切换到 Serverless，并且免费用户只能部署不超过32个文件，因此基于 OneIndex 的部署方式不再可行。另外一种方法是在 Heroku 上搭建，当然也可以使用 OneIndex，但是这时发现更好的程序是 OlaIndex，加密等功能更加强大，因此切换到了这个界面是。

## 版本选择

在写本文的时候，发现直接 OlaIndex 上的 master 分支代码存在一些问题，直接安装后遇到了 [issue-171](https://github.com/WangNingkai/OLAINDEX/issues/171) 的问题，而 release 页面下的 [3.2.1](https://github.com/WangNingkai/OLAINDEX/releases/tag/v3.2.1) 版本却缺乏密码设置，并不是 3.x 的最新版，最后发现 [release 分支](https://github.com/WangNingkai/OLAINDEX/tree/release) 才是 3.x 版本的可用分支，这里不得不吐槽下作者的混乱管理。

4.x 版本有较大更改，因此下文一些信息可能就不适用。

## 部署过程

网上也有一些相关的部署文档，但是试用下来却发现仍然有一定的错误，所以再详细记录下安装过程:

1. 构建本地环境

```
git clone --depth=1 -b release https://github.com/WangNingkai/OLAINDEX.git
cd OLAINDEX
rm -rf .git
composer install -vvv
```

2. 配置 heroku 环境

```
git init
echo web: vendor/bin/heroku-php-apache2 public/ > Procfile
git add -f composer.lock
heroku create
heroku buildpacks:set heroku/php
git push heroku master
heroku config:set APP_KEY=xxx
heroku config:set APP_URL=xxx
cp .env.example .env
```

修改 `.env` 文件中的环境变量，URL、KEY 信息，注释掉文件中的 redis 配置。

>APP_ENV=production  
>APP_KEY=XXX  
>APP_DEBUG=false  
>APP_URL=https://XXX.herokuapp.com

```
git add -f .env
git add .
git commit -m "init"
git push heroku master
```

执行 `heroku create` 后就能够看到自动分配的网址了，如果不满意可以执行 `heroku rename XXX` 来修改这个网址。

## 保存设置

heroku 的应用策略是默认是隔一段时间会自动休眠，这样一段时间没有访问后，应用会重新打开并需要重新初始化，网上有一个方法是运行命令：

`heroku run bash`

然后进入运行环境后，保存 `storage/app/config.json` 文件。但是我实际进入后，没有发现有什么保存的文件，因此失败。

因此我选择的方法是先在本地跑了一个 Docker 服务（避免搭建环境），然后初始化网站，注意因为是本地 Docker，所以没有 HTTPS 域名，需要利用中转 HTTPS 域名进行中转才行，这也是这种方法的一个小隐患。初始化完成后进入 Docker 后保存 `storage/app/config.json` 到项目目录中，再次提交。这样就算每次重置了应用，也可以利用保存的会话信息跳过初始化过程。

## 其他配置

部署完成后，发现虽然首页是 HTTPS 的，但是里面的链接都是 HTTP 链接，只不过打开后会自动跳转到对应的 HTTPS 链接而已，在密码框输入时 Firefox 也会提醒流量未加密。这是因为在使用负载均衡器的情况下，Laravel 接收到的来源是非加密流量，所以需要修改文件 `app\Http\Middleware\TrustProxies.php`：

```
<?php
namespace App\Http\Middleware;
use Illuminate\Http\Request;
use Fideloper\Proxy\TrustProxies as Middleware;
class TrustProxies extends Middleware
{
    protected $proxies = '*';
    protected $headers = Request:: HEADER_X_FORWARDED_AWS_ELB;
}
```

# 参考文档

* [Office 365 教育版](https://www.microsoft.com/zh-cn/education/products/office)

* [Office 365 开发人员计划订阅到期和续订](https://docs.microsoft.com/zh-cn/office/developer-program/subscription-expiration-and-renewal)

* [now.sh 免费部署 oneindex](https://www.ouyangsong.com/posts/43735/)

* [关于在 Heroku 部署时 HTTPS 的跳转](https://github.com/WangNingkai/OLAINDEX/issues/63)

* [getting-started-with-laravel#trusting-the-load-balancer](https://devcenter.heroku.com/articles/getting-started-with-laravel#trusting-the-load-balancer)

* [【sharepoint】解读onedrive每天流量限制](https://www.abbeyok.com/archives/266)