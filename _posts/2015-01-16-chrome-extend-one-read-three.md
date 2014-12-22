---
layout: post
title:  chrome插件one-read开发:提升
date:   2015-01-16 23:33:36
categories: oneeRead
tag: fontend
---
# 前言

>	点击这里你可以看到one-read "一览" chrome版的使用

代码在这里：[github code for one-read](https://github.com/icindy/one-read)


## 升级方式分析

*	通过定制version的方式进行用户通知更新

> 这种方式是用户手动更新，通过这种方式优点是可以满足用户的喜好，但是缺点就是用户需要卸载原本的程序后再安装

*	通过ajax调用远程代码方式进行更新

> 这种方式可以让用户没有丝毫感受的情况下进行更新，但是每次都要勇敢更新代码的方式，会对程序本省要求复杂，程序设计度也会复杂些。

下面我们会分析下两种方式。

## version标记方式更新

*	方式说明

1.再popup.html中建立标识“version”
2.编写后台代码，添加version管理
3.pop.js校验version
4.更新逻辑处理

*	具体实现

html代码

{% highlight html %}

	<!-- 标记目前版本号 -->
	<span id="version">0.1</span>

	<!--弹出提升更新框 -->
	<div class="alert alert-warning alert-dismissible version-alert" role="alert">
	  <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
	  <strong>Warning!</strong> 有更新，<a href="" target="_blank">点击这里更新</a>.
	</div>

{% endhighlight %}



javascript代码
{% highlight javascript %}

//请求版本号
commonAjaxFn("GET","http://onechrome.sinaapp.com/version.php","html",versionFn);
// 验证版本做出相应
function versionFn(data){
	var v = parseFloat($(data).text());
	var locV = parseFloat($("#version").text());
	if(locV < v){
		$(".version-alert").show();
	}

}
{% endhighlight %}



## ajax更新代码方式更新

*	方式说明

1.移除包内代码
2.重构ajax请求
3.特定请求代码
4。加载代码

*	具体实现

## 更好的方式？

>了解了上面两种方式以后，我们不难发现，如果能够采用两者的结合会是一个不错的方式。

* 	how to do it?
> 其实解决方案就是缓存。

1.我们主要采用ajax方案，但是同样制作version标记，
2.每次打开时执行缓存的文件
3.执行缓存中的version校验
4.检查是否version变动，如果变动，更新缓存

* let's do it



