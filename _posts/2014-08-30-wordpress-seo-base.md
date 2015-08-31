---
layout: post
title:  wordpress seo最基础准备
date:   2015-08-30 23:33:36
categories: seo
tag: fontend
---

# 前言

> 搭建了一些网站，一个也没有起来，所以就简单seo一下，通过站点的访问数据，目前有1/2的流量来自移动搜索，所以简单记录一下是应该很有必要。本文专特别针对wordpress站点

##网站基础
> 以下内容可以通过通过系统设置或者添加部分代码进行优化，详细可以具体搜索。

* a.站点标题
* b.站点关键词
* c.站点描述
* d.文章长尾词


##各大搜索引擎的提交入口


|| *搜索引擎* || *入口链接 (low)* || 
|| 百度搜索网站登录口 || [http://www.baidu.com/search/url_submit.html](http://www.baidu.com/search/url_submit.html) ||
|| 百度单个网页提交入口 || [http://zhanzhang.baidu.com/sitesubmit](http://zhanzhang.baidu.com/sitesubmit) ||
|| Google网站登录口 || [https://www.google.com/webmasters/tools/submit-url](https://www.google.com/webmasters/tools/submit-url) ||
|| 360搜索引擎登录入口 || [http://info.so.360.cn/site_submit.html](http://info.so.360.cn/site_submit.html) ||
|| 搜搜网站收录提交入口 || [http://www.soso.com/help/usb/urlsubmit.shtml](http://www.soso.com/help/usb/urlsubmit.shtml) ||
|| 搜狗网站收录提交入口 || [http://www.sogou.com/feedback/urlfeedback.php](http://www.sogou.com/feedback/urlfeedback.php) ||
|| 必应网站提交登录入口 || [http://www.bing.com/toolbox/submit-site-url](http://www.bing.com/toolbox/submit-site-url) ||


## 模版选择

> 模版的选择决定搜索引擎的索引

* a.模版的代码是否规范化，如标题h1 等明确表明标签内的内容含义

>简单测试方法： 安装模版后，去除所有的css，观察是否能够明确表明标题，内容等的层次

* b.响应式布局模版优先，分析cnzz的来源，会发现有将近1/3的流量来自移动端。

>目前搜索引擎更倾向与检索响应式布局的网站。


## 插件优化

>wordpress本身含有很多优化seo的插件

* a.插件1: All in One SEO Pack（wp插件中心）
* b.插件2:百度数据结构化插件http://zhanzhang.baidu.com/dataplug/index
* c.图片优化代码：

```php

//给文章图片自动添加alt和title信息

add_filter('the_content', 'imagesalt');

function imagesalt($content) {
       global $post;
       $pattern ="/<a(.*?)href=('|\")(.*?).(bmp|gif|jpeg|jpg|png)('|\")(.*?)>/i";
       $replacement = '<a$1href=$2$3.$4$5 alt="'.$post->post_title.'" title="'.$post->post_title.'"$6>';
       $content = preg_replace($pattern, $replacement, $content);
       return $content;
}

```

