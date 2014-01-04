---
layout: post
title: Maven 学习笔记(1)
description: maven 学习笔记
tags: [java, maven]
image: 
  feature: abstract-3.jpg
modified: 2013-12-11
comments: true
share: true
---

之前写Java程序一直都是用eclipse管理依赖项，最近学习了一下spring，发现世界的变化太快了，自己竟然不知道有maven这样的东西，虽然感觉很好，但是也有要过时的趋势，不禁把自己的未来吓尿了。。。言归正传，这里记录一下最近的学习心得，以便日后温习。

Maven这个东西很类似npm（我也不知道谁先谁后，也懒得查），总之是一个管理依赖项的东西，通过定义一个叫做pom.xml的文件，来自动的下载当前项目所需要的包，从而进行编译。个人觉得这个pom.xml与NodeJS中的package.json功能上基本相同，但是xml文档写起来嘛，就显然没有json那么舒服了，所以现在gradle这个东西比较流行（像json，简洁），不过本文就不谈gradle了，以后有时间再研究。

由于现在最新版本的eclipse-kepler原生支持了maven项目，使得上手稍微容易了那么一些。可以直接从eclipse建立新的Maven项目，然后观察Maven项目的基本结构，结构很清晰，就不赘述了。

# Maven的基本知识

一个Maven Project主要在于其方便的编译和运行：
编译：`mvn build`
打包：`mvn package` 
清理：`mvn clean`

Maven项目的pom.xml的编写过程是一个很痛苦的过程（个人感觉，因为XML文档太过冗长），所以Maven提供了一个非常方便的功能——继承，可以在自己的pom.xml文档中定义parent pom.xml，从而继承一些非常常规的设置，使自己的工作得到大大的简化。下面是我写spring项目的一个例子：

{% highlight xml%}
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>0.5.0.M6</version>
</parent>
{% endhighlight %}

同时，如果你用eclipse开发一个Maven项目，可以双击pom.xml文件，然后再右侧编辑器的下方找到几个tab，依次：Overview, Dependencies, Dependencies Hierachy, Effective POM and pom.xml。可以通过Effective POM来查看通过解析后世界的pom.xml文件是什么样子，相信你看过之后绝对不想自己把这个东西写一遍。。。。

# Maven的一些细节

## 依赖项打入Jar包

一个正常的项目不可能没有依赖，所以把所有依赖项都打入Jar包应该是一个理所应当的功能，Maven当然提供了，需要在你的pom.xml中加入如下一段代码

{% highlight xml%}
<plugin>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <descriptorRefs>
      <descriptorRef>jar-with-dependencies</descriptorRef>
    </descriptorRefs>
  <archive>
    <manifest>
      <mainClass></mainClass>
    </manifest>
  </archive>
  </configuration>
  <executions>
    <execution>
      <id>make-assembly</id>
      <phase>package</phase>
      <goals>
        <goal>single</goal>
      </goals>
   </execution>
  </executions>
</plugin>
{% endhighlight %}

今天先就写这些，以后有机会再写
