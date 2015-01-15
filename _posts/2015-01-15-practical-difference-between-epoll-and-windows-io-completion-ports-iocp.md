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

原文链接：http://www.ulduzsoft.com/2014/01/practical-difference-between-epoll-and-windows-io-completion-ports-iocp/

简介（Introduction）
==================

本文主要比较epoll和Windwos中I/O完成端口（IOCP）之间的不同。

本文可能会引起以下三种人的兴趣：

* 致力于开发高性能、跨平台网络服务的系统架构师
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


