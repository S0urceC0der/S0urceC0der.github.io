title: 使用 Graphviz 来画图
date: 2017-12-03 20:27:45
tags: []
---

# Graphviz 简介

符合程序员的以代码生成一切事物的思想，并且类似 HTML ，只定义结构，样式则可以通过第三方工具来进行美化。

例如 [oxygen](https://jeasonstudio.github.io/oxygen-demo/) 就是一个可以通过 Graphviz 代码来生成手绘风格图表的网站。

# 基本概念

`Node` 就是图的一个结点。

`Edge` 连接两个结点之前

`Graph` 定义了整个图片


# 工具教程

用过 NodeJS 的都了解，有一个 `supervisor` 命令能够监听目录，一旦发现文件发生改变后自动重启程序。发现 `dot` 命令并不存在自动监听生成图片的功能，因此改造了下，安装第三方工具 nodemon 来实现文件监听。

```
npm install -g nodemon
```

然后运行命令 `nodemon -e .gv -x "dot -Tpng -o test.png stack.gv"` ，便实现了 nodemon 监听指定后缀名的功能，然后执行指定的命令。可惜的是暂时没有找到将发生文件变化的文件名作为变量传入到命令中的方法。

# 参考

[Guide](https://graphviz.gitlab.io/_pages/pdf/dotguide.pdf)