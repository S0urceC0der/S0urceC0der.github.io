title: Node开发
date: 2014-12-12 15:26:22
tags:
---


本文的主要内容：使用Node Coffee-script开发一个网站

#### Mongoose

注意，Mongoose会对输入的Collection名字自动添加复数形式命名后存入到Mongodb中，例如user重命名为表users，具体的命名方法可以参考[Mongoose在创建Model时对Collection的命名策略](http://aiilive.blog.51cto.com/1925756/1405203)

解决方法是在创建Model的时候传入第三个参数作为实际的表名

在查询中，还发现在查询大量数据的时候会出现`Error: parseError occured`的错误提示，相关的错误可以参看[MongoDB parseError when using limit and sort](https://stackoverflow.com/questions/21767673/mongodb-parseerror-when-using-limit-and-sort)，大意是缺少索引，但是我明明已经添加了相关的索引的，不知道如何去解决，故改用Mongodb试下

