title: Hello World
date: 2014-11-20 22:19:23
tags:
---

这是我第二次使用github来发表博客。第一次的用户名暴露了太多，因此就使用了新的用户名:S0urceC0der，比较装逼吧，呵呵。

## 为什么使用Github发表博客

原因其实很简单，目前还没有心思打理一个主机，一般的博客网站编辑起来也比较麻烦，而Github提供了相当不错的静态网站访问功能。

之前自己曾短暂的使用jekyll在Github上搭建博客，但是因为jekyll是Ruby写的，自己心里多多少少有所抵触，最终也没有坚持下来。而现在使用的Hexo，因为是Node写得，之前曾经使用过一阵，因此也比较亲切，看着各种主题也比较多，够酷够帅，也不用太折腾，就使用上了。

写博客主要使用Markdown，一直以来都写的比较顺手，语法足够简单，真的是能够让人专注于内容写作，不用操心排版（虽然有时候要选择一个比较好的渲染邀请）。加上各种完美的字体与排版，比Word好多了，让人舒心。恐怕未来很长一段时间内，都是我写作的主要工具吧。

我也会把以前的几篇文章，逐步转移到现在的这个博客上的，算是增加点内容吧。

## 搭建Hexo

### Github设置

[Github Page](https://pages.github.com/)支持两种格式的静态页面:

1. User Site: 仓库名称为username.github.io，其中username是你自己的用户名，主要用于个人的博客页面，页面数据存放在主干上。对应的访问页面就是http://username.github.io

2. Project Site: 在指定的仓库下新建gh-pages分支存储页面，主要用于对某个项目的网页描述，对应的访问页面是http://username.github.io/repository

当然，我的目标是个人博客，因此就新建了与用户名相同的个人仓库，并在主干上存放对应的静态页面文件。

因为使用了Hexo生成静态页面，所以额外新建Source分支用于存储源代码（Hexo的工程），这样便可以在不同电脑随时随地检出进行修改，也实现了远程的文件备份。

注意需要在本地的gitignore添加如下内容，以便不上传无用的文件内容：

> public/       #Hexo生成的静态页面目录
> node_modules/ #node模块安装目录

### Git命令

以下是创建仓库时的一些命令：

`git config --global user.name 用户名`：设置用户名

`git config --global user.email 邮箱`：设置邮箱

`git branche source`：创建source分支

`git checkout source`： 检出source分支

`git checkout [file]`：回滚某个文件

`git fetch origin`：升级仓库

`git push origin source`或者`git push origin source:source`：将本地source分支推送到origin仓库的source分支中

## 写作

用Markdown语法写作，非常轻松

### 添加照片

将照片放到source/img文件夹内，然后使用 `![图片标题](/img/图片名称)`来显示

### 额外功能

除了标准的Markdown语法以外，还有新增了不少新的语法特性，这些语法特性，可以增强显示内容。

## 博客发表

`hexo n <title>`：发布博客

`hexo g`：生成博客

`hexo p`： 推送到github上

## 2015-03-16更新

昨天更新了一些软件，结果发现hexo生成的index.html是没有完全渲染过的，即仅包含ejs模板代码，几经搜索不知其解，便想着重装hexo，结果跑到官网一看，已经把包由`hexo`变成了`hexo-cli`，估计应该是版本升级有所变动。

在重新安装完毕后，却又发现没有了之前的`clean deploy`等指令，重新新建了一个空的工程，对比了下package.json，才发现`hexo`的各种`generator`、`render`都已经剥离了，必须重新安装到当前目录下才行。最终，覆盖了原来的package.json，运行`cnpm install`之后，再次敲入`hexo`帮助，发现已经有了各种额外的指令。

当一切都搞定后，才回过神来要去看下官网的[3.0 Changelog](https://github.com/hexojs/hexo/wiki/Breaking-Changes-in-Hexo-3.0)，的确在3.0版本中，更换了命令行，并进行了很多优化。文档中也提供了对应的[迁移指南](http://hexo.io/news/2015/03/05/hexo-3-0-released/#How_to_Update?)，自己还是过于懒惰不喜欢看文档，走了不该走的弯路。

最后思考了下，认为这种做法的优点是：

1. 框架与实现分离：剥离全局共用的指令（`-cli`库），`-cli`只提供最基本的功能，安装额外功能后，才具备额外功能。

2. 实现在不同版本间的共存：只需要在package.json文件中指定具体版本即可。

3. 减少了全局需要安装的内容

## Todo

<del>最好利用Github的教育优惠注册一个me域名</del>

修改主题风格，添加一些插件等等
