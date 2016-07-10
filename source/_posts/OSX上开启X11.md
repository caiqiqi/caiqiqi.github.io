---
title: OSX上开启X11
date: 2016-07-11 02:13:13
tags: X11
---

开始因为是搜ubuntu怎么开启X11 forwarding，然后看到很多说法是只需要安装server端的X11，后来看到stackOverflow里面说想要在OSX里面通过ssh到ubuntu虚拟机的化，还得在OSX里面安装一个X11client，然后按照[这个链接](https://support.apple.com/kb/DL1605?viewlocale=en_US&locale=en_US)找到的发现pmg安装之后发现这个软件只适用于OSX 10.7 于是我在苹果的官网搜索X11，发现了这个东西
![这里写图片描述](http://img.blog.csdn.net/20160510125509261)
然后就去安装这个`XQuartz`
https://support.apple.com/en-us/HT201341 </br>
![这里写图片描述](http://img.blog.csdn.net/20160510125617341) </br>
完成之后可以用XQuartz打开需要图形界面的程序，比如这个`NetAnim`
![这里写图片描述](http://img.blog.csdn.net/20160510194149324)
![这里写图片描述](http://img.blog.csdn.net/20160510194116114)
