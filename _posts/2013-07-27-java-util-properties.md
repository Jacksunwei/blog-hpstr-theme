---
layout: post
title: Java笔记(1) - java.util.Properties
description: Java笔记
tags: [java, java.util]
image: 
  feature: abstract-3.jpg
modified: 2013-07-27
comments: true
share: true
---

在Java程序的编写中，特别是基于命令行的后台程序，经常需要跟*系统设置*或者*用户设置*打交道，而且后台程序经常需要根据不同的命令行设置或者配置文件进行调整，因此，一个良好设计的**设置子系统**就会使得程序的可用性大大增加。

本质上讲，所谓的配置就是一序列键值对，Java Utility提供了一个用于管理这些键值对的简单的类java.util.Properties，这个类继承自Hashtable<Object,Object>，并提供了基础的getter/setter方法

{% highlight java %}
public synchronized Object setProperty(String key, String value);
public String getProperty(String key);
public String getProperty(String key, String defaultValue);
{% endhighlight %}

同时，这个类还提供了一个非常简单的载入配置的实现

{% highlight java %}
public synchronized void load(Reader reader) throws IOException;
public synchronized void load(InputStream inStream) throws IOException;
{% endhighlight %}

默认情况下，这个两个函数的工作方式是：以行为单位，每行为一个属性，取第一个分隔符（空格、冒号等）之前的字符串为键，之后一直到行尾的字符串为值。举个例子:
{% highlight bash %}
java 
-cp ./ 
-d ./bin
{% endhighlight %}


这个例子中，调用*load*函数之后会自动识别出三个属性
{% highlight bash %}
{
    "java": "",
    "-cp": "./",
    "-d": "./bin"
}
{% endhighlight %}

以上就是关于这个类的初步介绍，以后有新想法，再查缺补漏。
