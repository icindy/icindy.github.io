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
 >本文的相关代码：[点击查看demo的代码](https://github.com/icindy/node-webkit-demo)

本文会根据以前的文章进行实践，同样会变更一些地方。主要从以下几个方法介绍：
 1. 获取node-webkit
 1. 以我的博客为例制作一个客户端。
 1. windows下打包一个一键安装包

##引用

最近开发艾联的商家管理web后台，由于后台的效果采用了html5，为了照顾到所有低版本浏览器的商家，开始了无休止的兼容性调试，但是最终还是没有达到预期。

为了提供更好的体验，决定采用pc客户端的形势展示商家管理后台。
查了一下资料，发现有两款基于node的客户端打包工具
appjs：https://github.com/appjs/appjs
node-webkit: https://github.com/rogerwang/node-webkit
因为先看到的是node-webkit所以就采用了这种方式
作用：将web转化成原生桌面的应用（win，linux，mac）
步骤：
 1. 制作web页面
 1. 打包文件，参考[如何打包你的app](https://github.com/rogerwang/node-webkit/wiki/How-to-package-and-distribute-your-apps)
 1. win下可以通过Inno等软件制作安装包
问题：
 1. 64位运行：因为自己傻，一开始没看懂api，后来看懂了，直接把包放到目录下运行就好了
 1. 软件更新：这是我没有解决的地方，因为没办法直接去修改打包的内容，所以没办法去更新。我只好退而求其次，直接把

 我的地址放在了配置文件中。这样我就不用担心软件的更新了

最终的windows版就直接可以打包成了安装包。由于属于公司产品，所以没有办法开放我的源码，请见谅。

##获取node-webkit

 1. [git地址](https://github.com/rogerwang/node-webkit)
 1. [很好的中文手册&教程](http://www.cnblogs.com/xuanhun/tag/node-webkit/)


##实际例子

 >大家现在浏览的是我的博客，我们以这个博客为例子，分别进行封装。因为我的博客做过自适应，所以我们自己来把它变成一个小型的桌面应用。

 * 下载node-webkit 32位
[这里可以很好的下载各种版本的](https://github.com/rogerwang/node-webkit/#downloads)。我下载32位的。
 * 解压配置
解压你下载的文件包，在里面新建一个文件夹，我命名为我的博客名称“cindy”
![软件包图片](/images/post/node-webkit/image1.png)
 * 添加配置文件`package.json`
 我的配置文件很简单，因为我是远程加载的。

 {% highlight json %}
	{
  "main": "http://cindyfn.com",

  "name": "cindy-blog",

  "description": "会写ios的前端",

  "version": "0.1.0",

  "keywords": [ "web","IOS","nodejs","javascript","js","object-c","前端开发","ios开发" ],

  //定义windows表现
  "window": {

    "title": "cindy的博客",

    "icon": "logo.png",

    "toolbar": false,

    "frame": true,

    "width": 320,

    "height": 500

  }

}
 {% endhighlight %}

 * 打包运行
 >备：windows下运行可以将cindy文件夹拖拽到nw.exe中进行展示

 打包：在windows下打包cindy文件夹下文件为cindy.zip打包，并修改名字为cindy.nw
 打包exe: `copy /b nw.exe+cindy.nw cindy.exe`
 ![软件包图片2](/images/post/node-webkit/dos2.png)

 这样就会在文件夹下又一个cindy.exe，点击运行就会看到相应的效果。
 ![软件包图片3](/images/post/node-webkit/xg1.png)


 不过。你不要以为这样就大功告成了，你尝试把cindy.exe单独拿出来运行，貌似不能运行了。为什么？以为它的运行是依赖包内的chrome的，你单独拿出来当然不能运行了。不过别急，继续往下面看，教你如何打包安装包。

 > 切换到windows电脑。有点不习惯。

##windows下封装

 >趣：由于我目前用的是mac，没有win系统，所以把我尘封已久的电脑拿出来了，画质不好不要怪我。

 node-webkit官方建议使用的封装软件是Inno
 具体的使用方式可以参考这里[如何封装EXE安装程序](http://jingyan.baidu.com/article/36d6ed1f50ecfc1bcf4883aa.html)

 我已经按照这个步骤封装了我的博客exe，[参见这里cindysetup.exe](https://github.com/icindy/node-webkit-demo)

 ## 说在后面

 本文是针对windows进行封装，你也可以尝试使用mac，linux下的封装。希望你能够分享下。













