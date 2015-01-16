---
layout: post
title:  [译]Practical difference between epoll and Windows IO Completion Ports (IOCP)
description: 2014 review
tags: [iocp]
image: 
  feature: abstract-2.jpg
modified: 2015-01-15
comments: true
share: true
---

原文链接：[http://www.ulduzsoft.com/2014/01/practical-difference-between-epoll-and-windows-io-completion-ports-iocp/](http://www.ulduzsoft.com/2014/01/practical-difference-between-epoll-and-windows-io-completion-ports-iocp/)

目录
====

<!-- MarkdownTOC -->

- 简介（Introduction）
- 通知类型（Notification Type）

<!-- /MarkdownTOC -->


简介（Introduction）
==================

本文主要比较epoll和Windwos中I/O完成端口（IOCP）之间的不同。

本文可能会引起以下三种人的兴趣：

* 致力于开发*高性能*、*跨平台*网络服务的系统架构师
* 要将代码移植到这两个平台的朋友
* 已经熟悉其一，希望学习另外一个的求知欲甚强的朋友

当你需要支持高性能网络或者海量连接时，无论是epoll还是IOCP，都是非常有效的技术！
他们与其他的轮训（polling）方法的不同主要体现在一下几点：

* 他们不对描述符的总数量设置上限
* 当有大量的描述符时，他们的扩展性非常出色；与其他的轮训方法相比，添加一个描述符对它们
  来说代价非常小。
* 他们都非常适合基于线程池的模型，即用一个较小的线程池，来处理大量连接
* 如果你的客户端数量不多，那么他们通常并不高效、负担重甚至毫无用处。
  他们的主要用途是用来处理超过1000个并发连接的。

简而言之，这些技术是用来搭建能够处理巨量并发连接的网络服务器的。尽管如此，他们之间还是
有很多不同之处，了解他们的不同是非常重要的。

通知类型（Notification Type）
===========================
epoll和IOCP第一个重大的不同就是*何时收到通知*。

* epoll在文件描述符对“要求操作”（requeset operation）就绪时，发出通知。
* IOCP则在“要求操作”（request operatoion）完成/无法完成时，发出通知。

当使用epoll时，一个应用程序将：

* 决定要请求一个文件描述符的何种动作（读、写或又读又写）
* 使用*epoll_ctl*设置轮训掩码（polling mask）
* 调用*epoll_wait*，阻塞线程，直到有至少一个事件被触发。如果多个事件被触发，函数会返回
  尽量多的事件。
* 从`union data`获取事件数据指针。参见[这里](http://linux.die.net/man/2/epoll_wait)
* 如果在返回的event structure（`epoll_event`）中，对应的标志位被设定，那么就触发对应
  的操作（读、写或又读又写）
* 在所有操作完成后（应该是瞬间完成的），处理读入的数据或发送更多的数据
* 特别的，文件描述符很有可能被同时设置为读和写，以便程序能进行两种操作

当使用IOCP时，一个应用程序将：

* 使用非零*OVERLAPPED*结构体作为参数，对一个文件描述符（由*ReadFile*或*WriteFile*
  API获得）触发一个需要的动作。系统将这个操作入队，函数调用立即返回。
  （注：也许这个函数也能够瞬间完成，但是这并不影响整个逻辑，因为即便瞬间完成的操作，仍然
  会向完成端口投递消息）
* 调用*GetQueuedCompletionStatus()*，阻塞当前线程，直到一个操作完成并投递消息。即便有
  多个操作同时完成，效果也是一样，*GetQueuedCompletionStatus()*只会挑选一个返回。
* 利用completion key和*OVERLAPPED*结构体，找到事件数据指针。
* 处理读到的数据，或者发送更多数据
* 同一时刻，只有一个完成操作可以被获取。

epoll和IOCP在通知类型上的差异，使得我们可以轻易的使用epoll，外加一个独立的线程池，
来模拟IOCP。然后，反过来却并不容易。据我的了解，还没有比较容易的利用IOCP来模拟epoll的实
现，而且似乎很难在保证同样性能的前提下，实现用IOCP来模拟epoll。


