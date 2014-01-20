---
layout: post
title: 让eclipse menu bar 在ubuntu中正常显示
description: "解决eclipse在ubuntu13.10下menu不正常显示的问题"
modified: 2014-01-20
tags: [ubuntu, ubuntu 13.10, eclipse, eclipse kepler]
image:
  feature: abstract-10.jpg
comments: true
share: true
---

问题描述：在经历了很久虚拟机后，发现Windows的利用率简直低得可怜，于是下了狠心，直接抛弃了windows。
然后就遇到了第一个问题，eclipse在ubuntu 13.10下，menu bar没法正常显示，这就很坑爹了。

问题原因
--------
经过一番折腾和google，终于找到了原因，就是eclipse和ubuntu桌面的menu兼容不好，那就很简单了，
本着懒人的一贯精神，让eclipse直接弃用unity的menu bar就好了。

解决方法
-------
给eclipse添加一个默认的桌面选项，然后从ubuntu的快速搜索里启动就好了。

ubuntu管理的应用都在/usr/share/applications/文件夹下有个对应的以"desktop"为后缀名的文件，
随便找个文件复制一下，改一改就好了，比如我这里用的：

{% highlight ini %}
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse J2EE
Comment=Eclipse J2EE
Exec=env UBUNTU_MENUPROXY=0 /home/jack/Softwares/eclipse-jee/eclipse
Icon=/home/jack/Softwares/eclipse-jee/icon.xpm
Terminal=false
StartupNotify=true
Type=Application
Categories=Development;
{% endhighlight %}

关键的地方就是`UBUNTU_MENUPROXY=0`，其他的地方就看着办吧。然后从quick search里直接搜`eclipse-jee`打开就好了。

最后
---
先弄了个简单能用的方法，不一定是最合适的，有更合适的欢迎留言。
