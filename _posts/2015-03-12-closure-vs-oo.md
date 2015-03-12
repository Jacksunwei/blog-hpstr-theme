---
layout: post
title: 闭包与OO
description: closure vs. oo in programming
tags: [closure, oo]
image: 
  feature: abstract-3.jpg
modified: 2015-03-12
comments: true
share: true
---

最近又有点空，翻出了[《Linux多线程服务端编程：使用muduo C++网络库》](http://www.duokan.com/book/76600)
随便看看，又看到了一些而之前没有注意过的问题，这要从我之前写Java的时候，一直存在的一个
疑问开始说起。

在Java多线程中，如果要是实现一个线程，有两种方式：

* 继承自`Thread`类，重载`Thread`类中的`run()`方法
* 使用一个`Runnable`的对象new一个`Thread`，直接调用start()启动线程

于是，问题来了：

    为什么Java要设计这两种方式？在什么场景下会有不同？

对于以上这个问题，这次在书中，我看到了一定的答案。

库的接口设计
===========
要回答上面的问题，首先要从库的接口设计说起。
传统上，库的接口通过虚函数的形式给出。库的用户只需要重在这些虚函数，就将业务逻辑注入到
库中，达到利用库的目的。

这对于习惯使用OO的开发人员来说，是非常常规的思路，即使用了“多态”。但是继承本身就是一种
非常强的耦合，如果现在的需求在未来需要改变，那么很可能面临着一种牵一发动全身的窘境。

那么如何在不使用继承的情况下，又使用“多态”呢？其实，多态也可以理解为动态绑定，虚函数只是
多态的一种表现形式，我们完全可以不使用虚函数，也实现多态。闭包就是一个不错的选择。
闭包可以天然地包含一段逻辑和一组名字绑定，这就是有了一个类的全部功能（成员变量和成员方法）。
在编写业务逻辑时，我们可以动态的生成闭包，并把它交给库的接口，由库完成它应该完成的功能，
如此就可以即摆脱了继承，由不丢失“多态”。

下面用一个具体点的例子说话：

传统方法中，我们要

{% highlight java %}

public class ThreadA extends Thread {
    private Data data;
    
    public void run() {
        // do something with data
    }
}

// usage code
ThreadA threadA = new ThreadA();
threadA.start();

{% endhighlight %}

而如果我们使用闭包的话，就可以用下面的方式

{% highlight java %}

// usage code
Data data = new Data();
Thread threadB = new Thread(new Runnable() {
    @Override
    public void run() {
        // do something with data
    }
});
threadB.start();

{% endhighlight %}

小结
====

在Java中，这两种方式看似稀松平常，但是背后却是两种不同的设计思想：一种是以继承的方式将
用户的逻辑注入库中；一种是以闭包的形式将用户的逻辑注入库中。在完成相同功能的情况下，后一种
可以带来更加轻便的设计。如果稍加类比，这些思想也可以带进其他的项目中，或者其他语言中。
