---
layout: post
title:  征服ReactiveCocoa 1 - 核心思想(别看，正在编写)
date:   2014-12-01 23:33:36
categories: ReactiveCocoa
tag: ios
---

##前言

>什么是ReactiveCocoa？查看下面的一系列文章了解，再开始下面的文章前，请确保您对MVVM模式和FER模式有了自己比较深的理解。感谢大神们原创或者翻译的文章，对理解RAC很有帮助。我下面的文章应该算是很多重复了大神们的观点和想法，再次感谢。

推荐以下文章，大家也可以自行搜索查看

###简介篇

 1. [MVVM启示录](http://www.infoq.com/cn/articles/mvvm-revelation/)
 1. [RAC IOS开发新框架](http://www.infoq.com/cn/articles/reactivecocoa-ios-new-develop-framework)
 1. [wiki MVVM](http://en.wikipedia.org/wiki/Model_View_ViewModel)
 1. [ReactiveCocoa与Functional Reactive Programmin](http://limboy.me/ios/2013/06/19/frp-reactivecocoa.html)

###理解篇

 1. [说说ReactiveCocoa 2](http://limboy.me/ios/2013/12/27/reactivecocoa-2.html)
 1. [基于AFNetworking2.0和ReactiveCocoa2.1的iOS REST Client](http://limboy.me/ios/2014/01/05/ios-rest-client-implementation.html)
 1. [ReactiveCocoa2实战](http://limboy.me/ios/2014/06/06/deep-into-reactivecocoa2.html)

###实践篇
 
 1. [MVVM指南一：Flickr搜索实例](http://southpeak.github.io/blog/2014/08/08/mvvmzhi-nan-yi-:flickrsou-suo-shi-li/)
 1. [MVVM指南二：Flickr搜索深入](http://southpeak.github.io/blog/2014/08/12/mvvmzhi-nan-er-:flickrsou-suo-shen-ru/)
 1. [ReactiveCocoa指南一：信号](http://southpeak.github.io/blog/2014/08/02/reactivecocoazhi-nan-%5B%3F%5D-:xin-hao/)
 1. [ReactiveCocoa指南二：Twitter搜索实例](http://southpeak.github.io/blog/2014/08/02/reactivecocoazhi-nan-er-:twittersou-suo-shi-li/)

##开始

从现在，我已经假设读者大致理解了什么是mvvm以及RAC的基本思想。以下的文章主要根据

