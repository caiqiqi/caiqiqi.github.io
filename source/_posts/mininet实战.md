---
title: mininet实战
date: 2016-07-11 02:13:14
tags: [SDN, mininet]
---

先来一个图。</br>
![这里写图片描述](http://img.blog.csdn.net/20160512021644309)
先`sudo mn`模拟默认的环境。一个Controller c0; 一个OVSSwitch s1; 然后两个Host h1,h2。</br>
在dump中可以看到其实s1就是本机(ubuntu虚拟机系统)；c0是占用本机的某个端口，在某进程运行；而h1和h2是用两个进程来实现的。
不要试图去抓s1的包，因为它这会捕获本机的各种流量。
![这里写图片描述](http://img.blog.csdn.net/20160512022104435)
而应该在h1或者h2监听流量。
先在h1上`tcpdump -n`来监听通过h1的流量。
然后在mininet命令行里`h1 ping h2`来产生通信流量。
抓包结果如下：</br>
![这里写图片描述](http://img.blog.csdn.net/20160512022537843)
![这里写图片描述](http://img.blog.csdn.net/20160512022511075)
这个用XQuartz打开的窗口不能拉伸，所以不得不截两张图。

然后用`iperf` 测h1与h2之间的TCP带宽和`iperfudp`测UDP带宽。哇，我的TCP带宽居然这么高『17.1 Gbits/sec』，跟CPU有关吗
![这里写图片描述](http://img.blog.csdn.net/20160512023448175)

Edited on 5/14 03:13--------------------------------</br>
![这里写图片描述](http://img.blog.csdn.net/20160514031526782)
创建单OpenvSwitch然后四个主机的网络，h4作为http服务器，h1通过wget访问h4  PS：![视频教程](https://www.youtube.com/watch?v=l25Ukkmk6Sk)讲得好！
![这里写图片描述](http://img.blog.csdn.net/20160514031613377)
![这里写图片描述](http://img.blog.csdn.net/20160514031636221)

