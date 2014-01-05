---
title: 优化页面载入的实践
description: 页面优化
layout: post
image:
  feature: abstract-3.jpg
tags: [html, javascript]
modified: 2013-07-20
comments: true
share: true
---
#### 1. 将CSS代码在JavaScript代码之前加载，因为除非所有CSS都被载入，否则页面不可能正常显示

例如：

{% highlight html %}
<link href="style.css" rel="stylesheet" type="text/css" />;
<script type="text/javascript" src="jquery.min.js"></script>
{% endhighlight %}

#### 2. 对于只负责交互的JavaScript代码，可以加async属性，从而加快页面加载素的

例如：

{% highlight html %}
<script async type="text/javascript" src="jquery.min.js"></script>
{% endhighlight %}

#### 3. 删除不必要的页面请求，如图片、404请求等

#### 4. 使用JavaScript压缩工具

如：[Google Closure Compiler](http://closure-compiler.appspot.com/)

#### 5. 可以用Google Chrome PageSpeed测试你的网页加载速度
