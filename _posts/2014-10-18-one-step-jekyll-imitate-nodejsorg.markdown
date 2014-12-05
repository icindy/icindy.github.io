---
layout: post
title:  jekyll搭建－模仿nodejs.org
date:   2014-10-18 23:33:36
categories: jekyll
tag: html
---

前言
---

>你如果不问我最近怎么没有更新文章？<br>
>说明你没有关心我的帖子，看不到程序员的熬夜加班嘛！<br>
>最近加班有点多，所以我没有更新，当然我也在思考下一步干什么？<br>

>在正式开始今天的文章前，说下最近这N篇文章，我们要干什么？因为我还没有打算好几篇文章能写完，因为白天要认真干活，晚上才能写文章，生命下，现在的时间是晚上11:24分,所以按照计划，我每天安排一个小时的写作和编辑代码，应该能够在一周内完成我想要做的事情。

我写这些文章的目的：以[nodejs官方网站](http://www.nodejs.org/)为配色方案和格调，制作一个自己的 jekyll的博客，并挂载在github上发布
那我们要干什么那？请看下面的清单。

- 1.第一个小时：熟悉jekyll的框架，并构建一个基础框架
- 2.第二个小时：分析nodejs官方网站
- 3.第三个小时：配色和构架方案
- 4.第三个小时：构造自己网站的基础框架
- 5.第四－七个小时：完成网站的编码
- 6.最八个小时：部署到github上去，并上线
- 7.倒数第二个小时：分析下markdown的语法
- 8.最后一个小时：总结下在这个过程中我们的经验和教训
———————————————————
今天我们来完成第一步，部署一个jekyll的基础框架
ok，看不懂英文哇－来个[中文的网站jekyll中文网站](http://jekyllcn.com/)


jekyll安装步骤

```base
$ gem install jekyll
~ $ jekyll new my-awesome-site
~ $ cd my-awesome-site
~/my-awesome-site $ jekyll serve
# => Now browse to http://localhost:4000
```
