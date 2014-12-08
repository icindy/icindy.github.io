---
layout: post
title:  jekyll搭建3－配色与基础框架
date:   2014-10-20 23:33:36
categories: jekyll
tag: fontend
---
前言
---
>前面两个小时我们完成了我们任务分配和网站分析工作，今天我们要着手对我们想要做的网站进行基础性的配色和框架搭建工作

一：配色及字体搭配
---

* 1.配色方案

强调色#80bd01
简介色#ccc;
小说明rgba(153,153,153,.9);
正文#fff未选中导航色#ccc选中导航色#80bd01
导航hove下划线超链接#80bd01

* 2.字体方案
{% highlight css %}
font-size:16px;
font-family: Georgia,"Times New Roman",Times,"Hiragino Sans GB","Helvetica Neue",Helvetica,Arial,sans-serif;
大:16px
中:14px
小:12px
h1:30px
h2:25px
h3:20px
h4:15px
{% endhighlight %}

![font](/images/post/jekyll/jekyll-font.png)


* 3.代码展示方案

具体方案详见:[pygments](http://pygments.org/)

具体配色方案css：[具体css点击查看](https://github.com/icindy/icindy.github.io/blob/master/css/syntax.css)

需要注意的是，如果想要代码高亮呈现效果，需要jekyll的运行环境
![jekyll-code](/images/post/jekyll/jekyll-code.png)

* 4.评论效果

两种评论：

#### disqus

![jekyll-disqus](/images/post/jekyll/jekyll-disqus.png)

#### 多说
![jekyll-duoshuo](/images/post/jekyll/jekyll-duoshuo.png)

可能大家的需求不一样，所以选择不一样，我在最初效果上将两者都加在了上面，但是我最后回选择“多说”，毕竟我们经常使用的还是我们常见的社交软件

* 5.具体效果

具体代码:[git cindy](https://github.com/icindy/icindy.github.io)

具体展示效果：[点击查看](http://icindy.github.io/)
