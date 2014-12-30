---
layout: post
title:  node-webkit实践－一键安装
date:   2015-02-01 23:33:36
categories: node-webkit
tag: fontend
---

#前言
 >很久以前我简单的写过关于node-webkit的使用,但是我在更换博客的时候丢弃了，我在下面的文章把它引用一遍。
 >node-webkit其实在很多地方用处很大，请自行发散。

本文会根据以前的文章进行实践，同样会变更一些地方。主要从以下几个方法介绍：
 1.获取node-webkit
 1.以我的博客为例制作一个客户端。
 1.windows下打包一个一键安装包

##引用

最近开发艾联的商家管理web后台，由于后台的效果采用了html5，为了照顾到所有低版本浏览器的商家，开始了无休止的兼容性调试，但是最终还是没有达到预期。

为了提供更好的体验，决定采用pc客户端的形势展示商家管理后台。
查了一下资料，发现有两款基于node的客户端打包工具
appjs：https://github.com/appjs/appjs
node-webkit: https://github.com/rogerwang/node-webkit
因为先看到的是node-webkit所以就采用了这种方式
作用：将web转化成原生桌面的应用（win，linux，mac）
步骤：
 1.制作web页面
 1.打包文件，参考[如何打包你的app](https://github.com/rogerwang/node-webkit/wiki/How-to-package-and-distribute-your-apps)
 1.win下可以通过Inno等软件制作安装包
问题：
 1.64位运行：因为自己傻，一开始没看懂api，后来看懂了，直接把包放到目录下运行就好了
 1.软件更新：这是我没有解决的地方，因为没办法直接去修改打包的内容，所以没办法去更新。我只好退而求其次，直接把我的地址放在了配置文件中。这样我就不用担心软件的更新了

最终的windows版就直接可以打包成了安装包。由于属于公司产品，所以没有办法开放我的源码，请见谅。

##获取node-webkit

 1.[git地址](https://github.com/rogerwang/node-webkit)
 1.[很好的中文手册&教程](http://www.cnblogs.com/xuanhun/tag/node-webkit/)


##实际例子

 >大家现在浏览的是我的博客，我们以这个博客为例子，分别进行封装。因为我的博客做过自适应，所以我们自己来把它变成一个小型的桌面应用。


##windows下打包

 >趣：由于我目前用的是mac，没有win系统，所以把我尘封已久的电脑拿出来了，画质不好不要怪我。













