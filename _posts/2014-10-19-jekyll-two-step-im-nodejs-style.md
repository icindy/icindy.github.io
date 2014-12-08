---
layout: post
title:  jekyll搭建2－分析指定的网站nodejs.org
date:   2014-10-19 23:33:36
categories: jekyll
tag: fontend
---

> 如果你不知道这个是干什么的，请看[模仿:第一小时]

> 原本想使用jekyll的别人家的模版，但是总是找不到自己喜欢的，而且如果使用别人家的模版我就不能够根据自己的想法随意的扩充自己的功能，既然这样，回头想想，我擦，原来我是前端工程师啊，哈哈，那就自己写一个吧。

>鉴于自己本身的PS技术和审美观，那么，我不得不借鉴下其他网站的风格。浏览了一些网站后，最终决定拿nodejs官方网站下手。
原因有如下几点

- 1.风格简单，不臃肿
- 2.习惯网站的配色风格，带点coder风格
- 3.图片素材比较少，可以有效的加快访问速度
- 4.细节考虑周到（后面我会说到这一点）
let’s go!不过下面的分析需要你有点想像力。
注意这个地方我们只是使用风格，布局和构造试图我们会在下个小时一起分析


一：粗分析我们要参考的网站
---

这个过程中，我们针对每一个页面心里先有个预期，我们分析的每个页面对应我们将要产出的哪个网站。我很清楚我将要做的博客含有主页｜文章列表页｜关于｜项目等

这个过程你可能会用到工具
1.图片分析工具，我使用的是“markman”
2.网页调试，我使用的chrome
我使用markdown在图片上标示了所有的颜色和布局分类

* 1.主页面分析

这个页面我想要转换成博客的主页，但是可能需要改造的部分比较多，先自行脑补，同时在图上吧该标记的颜色和布局标示出来
![main](/images/post/jekyll/nodejs-main.png)

* 2.文章列表页分析

这个页面我想要转换成博客的列表页面，主要采用布局一级配色
![post](/images/post/jekyll/nodejs-post.png)

* 3.文章页面分析

这个页面我想要转换成博客的文章页面，主要采用布局一级配色
![page](/images/post/jekyll/nodejs-page.png)

* 4.项目成品页面分析

这个页面我想要转换成博客的项目页面，主要采用布局一级配色
![project](/images/post/jekyll/nodejs-project.png)

* 5.开源项目页面分析

这个页面我想要转换成博客的开源项目页面，主要采用布局一级配色
![open](/images/post/jekyll/nodejs-open.png)

二：粗分析我们要参考的网站交互
---
如果你仔细看下这个网站内，有很多地方有细小的交互效果，因为不好表述，所以举个例子

看到主页http://nodejs.org/那个大大的install按钮，看看当鼠标移动到上面的效果
![button](/images/post/jekyll/nodejs-button.png)
{% highlight css %}
#intro .downloadbutton {
    background-color: #80bd01;
    width: 220px;
    font-size: 30px;
    display: block;
    margin: 30px auto 0px auto;
    font-family: "din-condensed-web","Arial Narrow",Arial,sans-serif;
    font-style: normal;
    font-weight: 400;
    text-transform: uppercase;
    padding-top: 12px;
    padding-bottom: 10px;
    }
{% endhighlight %}
所以我们要注意细节，这样才能有干净的感觉