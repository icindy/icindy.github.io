---
layout: post
title:  "“钓鱼网站”5分钟速成和简单原理"
date:   2014-10-14 23:33:36
categories: post
tag: html
---

<div class="entry-content">
		<p>对于做安全的大神，请安静的做你的美男子，不要往下看了。<br>
<strong>适合本文的读者有以下特点：</strong></p>
<ul>
<li>1.曾经有qq号被盗，或者其他网站上登录的账号被盗经验，但是不知道怎么丢失账号的</li>
<li>2.想做一个钓鱼网站的（不过这个，你会受到道德的惩罚，如果滩上大的，你要受到法律的严惩）</li>
<li>3.网页小白，和想做工程学黑客的。</li>
<li>4.无聊的</li>
</ul>
<p><strong>ok，开始我们的5分钟，以做qq网站的“钓鱼网站”为例子</strong><br>
===&gt;<a href="http://cindyfn.com/qq/qq.html" target="_blank">点击这里进入我下面做好的“钓鱼网站”</a><br>
####请放心，我在里面没有加入任何传输你账号的信息，如果不放心，输入账号密码时</p>
<p><a href="http://cindyfn.com/wp-content/uploads/2014/10/最终效果.png"><img class="alignnone size-full wp-image-225" src="http://cindyfn.com/wp-content/uploads/2014/10/最终效果.png" alt="最终效果" width="800" height="434"></a>可以输入假冒的</p>
<p>===&gt;<a href="https://github.com/icindy/diaoyu" target="_blank">代码放在了git上，点击访问</a></p>
<p><span id="more-216"></span></p>
<p>－－－－－－－－－－－－－<br>
******基础 伪造网站*****<br>
－－－－－－－－－－－－－<br>
你不需要UI，编码等等，你要做的就是copy。<br>
可以跟我一起做：<br>
1.打开qq的登录界面<a href="http://i.qq.com" target="_blank">点击打开</a><br>
2.点击鼠标右键，选择审查元素，copy代码（chrome,firefox其他的浏览器如果没有审查元素，可以查看源代码）</p>
<p><a href="http://cindyfn.com/wp-content/uploads/2014/10/1.png"><img class="alignnone size-full wp-image-221" src="http://cindyfn.com/wp-content/uploads/2014/10/1.png" alt="1" width="800" height="569"></a><br>
3.使用文本编辑器或者代码编辑器，建立一个qq.html(注意更改你文件的后缀为html)，保存双击浏览，应该和上面的网站一样的</p>
<p><a href="http://cindyfn.com/wp-content/uploads/2014/10/2.png"><img class="alignnone size-full wp-image-222" src="http://cindyfn.com/wp-content/uploads/2014/10/2.png" alt="2" width="800" height="577"></a></p>
<p>－－－－－－－－－－－－－<br>
******更改 获取数据*****<br>
－－－－－－－－－－－－－<br>
1.发现数据form在iframe中，那就再按照上面“基础部分仿造”，审查代码－》copy－》新建login.html</p>
<p><a href="http://cindyfn.com/wp-content/uploads/2014/10/3.png"><img class="alignnone size-full wp-image-223" src="http://cindyfn.com/wp-content/uploads/2014/10/3.png" alt="3" width="800" height="479"></a><br>
2.将qq.html中的iframe的src改称-&gt;login.html<br>
3.分析代码，看到点击按钮提交事件，只需要改造这一部分，<br>
看我代码：<br>
<code><br>
function getUserInfo(){<br>
var qq_account = document.getElementById("u").value;<br>
var pass = document.getElementById("p").value;<br>
alert("如果你真的这样做了,那么你就上当了：你的qq号："+qq_account+" "+"你的密码是：" + pass);<br>
return false;<br>
}<br>
</code><br>
4.稍微修饰以下：去掉验证码，设置其对应的div display：none；去除id等基本的东西完成了。<br>
5.你如果懂得一些代码，可以继续修改里面的东西让他变得完善起来<br>
6.再表单中，让账号和密码存储到你的数据库里，那么你就可以看到全部了。</p>
<p>这样，你的网站以及基本上和qq的网站相似度再80%了，对于骗取一般小白的qq号，应该可以了<br>
－－－－－－－－－－－－－<br>
******高级 工程学*****<br>
－－－－－－－－－－－－－<br>
涉及到工程学的黑客，要把握住用户的心里，<br>
1.选择容易混淆的域名，如官方的域名为：qq.com，你如果永远qqxxx.com的域名。一般人就不会看出来了。<br>
2.选择模拟第三方登录的方式，现在第三方登录很常见，大家很习惯，所以这种方式不会引起疑心。</p>
<p>还有其他的，你比我强，你应该会想到。</p>
<p>－－－－－－－－－－－－－<br>
******最后 不要被骗，不要骗人*****<br>
－－－－－－－－－－－－－<br>
写这个原因是很多朋友都有被钓鱼的经历，明白这些钓鱼网站的仿真程度和方法，你可以清楚的识别那些是钓鱼网站。<br>
如果你是骗人的，我劝你，还是早日收手。</p>
<p>好了，有什么要交流的，光顾我的博客</p>
			</div>