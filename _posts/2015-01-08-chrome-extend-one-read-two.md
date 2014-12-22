---
layout: post
title:  chrome插件one-read开发:细节
date:   2015-01-01 23:33:36
categories: oneeRead
tag: fontend
---

# 前言

>	点击这里你可以看到one-read "一览" chrome版的使用

## manifest文件
> 这里有详细关于manifest的介绍文档，[点击访问](http://open.chrome.360.cn/extension_dev/manifest.html)

{% highlight json %}
	{
	// 必须的字段
	  "name": "one-read",
	  "version": "0.1",
	  "manifest_version": 2,
	  // 建议提供的字段
	  "description": "内容聚合",
	  "icons": { 
	    "16": "icons/icon16.png",             
	    "48": "icons/icon48.png",            
	    "128": "icons/icon128.png" 
	  },
	  "browser_action": {
	    "default_icon": "icons/StatusIcon.png", // optional 
	    "default_title": "一览",   // optional; shown in tooltip 
	    "default_popup": "popup.html",       // optional 
	    "scripts": ["js/jquery.min.js","js/bootstrap.min.js","js/pop/popup.js"]
	  },



	  "permissions": [
	    "http://*/",
	    "http://jandan.net/feed",
	    "http://xueqiu.com/hots/topic/rss",
	    "http://mindstore.io/",
	    "http://segmentfault.com/blogs",
	    "http://www.zhihu.com/explore",
	    "http://solidot.org.feedsportal.com/c/33236/f/556826/index.rss",
	    "http://www.jianshu.com/",
	    "http://next.36kr.com/posts"
	  ]
	}  
{% endhighlight %}

着重介绍下permissions的属性，如果你不声明的话，你将无法获取到你想要获取的内容


## popup页面

>popup作为我们唯一加入的页面。这里是我们主要的面向用户的页面，这个页面会被渲染，同时展示在用户面前，因为有“一览”的界面，所以我只做了一点点的改动。关于pop页面，你可以[查看这里获取帮助](http://open.chrome.360.cn/extension_dev/browserAction.html#popups)

<div class="row">
	<div class="col-xs-6">![原始](/images/post/one/yilanori.png)</div>
	<div class="col-xs-6">![我的](/images/post/one/meyilan.png)</div>
</div>

## pop.js



## 整体

*	bootstrap作为前端框架
*	jquery作为js框架
*	360扩展文档作为支持文档