---
layout: post
title:  jekyll搭建6－绑定域名，启用。
date:   2014-10-22 23:33:36
categories: jekyll
tag: fontend
---
前言
---
> 经过了很长时间的拖延，最终还是将域名绑定了。整个过程，我用了六篇文章进行记录，希望能够给大家一定的帮助。这也是这个系列的最后的一篇文章。关于如何对github绑定自定义的域名。

一、创建“CNAME”文件
---
>	这一个比较简单，进入你的repo，然后创建文件“CNAME”(注意，没有后缀)
>	例如我的域名：cindyfn.com
你可以在文件中写入：
{% highlight css %}
	cindyfn.com
{% endhighlight %}

二、解析域名
---
>	进入你的域名提供商管理页面，我的是万网。
>	


-	1.建立A指向：

A  || @	|| 默认 ||	192.30.252.154
A  || @	|| 默认 ||	192.30.252.153


<div class="alert alert-danger" role="alert"><span class="glyphicon glyphicon-star">⚠:两个ip(192.30.252.154/192.30.252.153)是固定的。</span></div>

-	2.建立CNAME指向：

CNAME || www || 默认	|| icindy.github.io.

<div class="row">
	<div class="col-md-12">
		<div class="alert alert-danger" role="alert"><span class="glyphicon glyphicon-star">⚠指向的CNAME icindy.github.io后面要多加一个“.”</span></div>
	</div>
</div>

三、注意事项
---
<div class="row">
	<div class="col-md-12">
		<div class="alert alert-danger" role="alert"><span class="glyphicon glyphicon-star">⚠:两个ip(192.30.252.154/192.30.252.153)是固定的。</span></div>
	</div>
</div>

<div class="row">
	<div class="col-md-12">
		<div class="alert alert-danger" role="alert"><span class="glyphicon glyphicon-star">⚠指向的CNAME icindy.github.io后面要多加一个“.”</span></div>
	</div>
</div>


<div class="row">
	<div class="col-md-12">
		<div class="alert alert-danger" role="alert"><span class="glyphicon glyphicon-star">⚠:设置完成后，等待10分钟后才会生效，请耐心等待</span></div>
	</div>
</div>



>如果有什么问题，请在下面留言说明，谢谢。






