---
layout: post
title:  chrome插件one-read开发:细节
date:   2015-01-08 23:33:36
categories: oneeRead
tag: fontend
---

# 前言

>	点击这里你可以看到one-read "一览" chrome版的使用

代码在这里：[github code for one-read](https://github.com/icindy/one-read)

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

![原始](/images/post/one/yilanori.png)
![我的](/images/post/one/meyilan.png)

## pop.js

>这里是我们的主要pop.js,这里会主要负责我们的主要逻辑，包括ajax实现，xml的解析，小偷程序的实现。

*	ajax的实现我做了少量的改动

{% highlight javascript %}
	// common function
	//ajax发送执行的公共函数
	function commonAjaxFn(ajaxType, ajaxUrl, ajaxDataType, successFn){
		$.ajax({
		        type: ajaxType,
		        url: ajaxUrl,
		        dataType: ajaxDataType,
		        beforeSend: ajaxBeforeFn,
		        success: function(data){successFn(data)},
		        error: ajaxErrorFn,
		        complete: ajaxCompleteFn
	    });
	}
	function ajaxFn(data){

	}

	//ajax执行前的公共函数
	function ajaxBeforeFn(){
		$(".pop-div").show();
		$(".net-ok").hide();
		$(".spinner").show();       
	}

	//ajax执行完成后的公共函数
	function ajaxCompleteFn(){
		$(".spinner").hide();
		$(".net-ok").show();
		$(".pop-div").hide();
	}

	//ajax执行后错误的公共函数
	function ajaxErrorFn(){

	}
{% endhighlight %}

*	xml解析：以煎蛋为例子

点击按钮后的代码
{% highlight javascript %}
	//jianshu
	$("#jianshu-btn").on('click',function(){
		$("#nowDot").css({'left':'166px'});
		$("#jianshu-page").html("");
		commonAjaxFn("GET","http://www.jianshu.com/","html",jianshuFn);
	    return false;
	});
{% endhighlight %}

或许执行的代码，解析xml
{% highlight javascript %}
	//煎蛋函数
	function jandanFn(data){
		var ulHtml="";

		$(data).find("channel").find("item").each(function(index, ele) {
			if (index > 9) { return false};
			var title = $(ele).find("title").text();
			var link = $(ele).find("link").text();
			var  des = $(ele).find("description").text();
			var commentNum = $(ele).find("slashComments").text();
			var liHtml = '<li><a target="_blank" href="'+link
						+'" title="'+title+'" "><p class="page-title">'+title
						+'</p>'
						+'<p class="page-brief">'+commentNum+' 条评论 | '+des+'</p></a></li>';
			ulHtml += liHtml;
		});

		$(".cat-list ul").hide();
		$("#jandan-page").html(ulHtml).show();
	}
{% endhighlight %}

*	“小偷程序”的实现（针对没有xml）

{% highlight javascript %}
	//mindstoreFn
	function mindstoreFn(data){
		var ulHtml="";

		$(data).find("#mind-list").find("ul").eq(0).find("li").each(function(index, ele) {
			if (index > 9) { return false};
			var title = $(ele).find(".mind-title>a").text();
			var link = $(ele).find(".mind-title>a").attr("href");

			var  itemString = $(ele).html();
			var zanNum = $(ele).find(".vote-total").text();
			var mindDes = $(ele).find(".mind-des").text();

			var liHtml = '<li><a target="_blank" href="'+link
						+'" title="'+title+'" "><p class="page-title">'+title
						+'</p>'
						+'<p class="page-brief">'+zanNum+' 条支持 | '+mindDes+'</p></a></li>';
			ulHtml += liHtml;
		});
		
		$(".cat-list ul").hide();
		$("#mindstore-page").html(ulHtml).show();

	}
{% endhighlight %}

## 整体

*	bootstrap作为前端框架
*	jquery作为js框架
*	360扩展文档作为支持文档