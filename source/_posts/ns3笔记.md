---
title: ns3笔记
date: 2016-07-11 03:04:37
tags: [SDN, ns3, C++]
---

##Links
ns-3的API：</br>
https://www.nsnam.org/docs/release/3.17/doxygen/
一些学习的视频：</br>
[ns3 overview](https://www.youtube.com/watch?v=FFNvFzSmgIg)</br>

[ns3share's channel](https://www.youtube.com/user/ns3share/videos)</br>
[tutorial-ns3](https://www.youtube.com/watch?v=AU7ZjBvQaBk)</br>
{% youtube AU7ZjBvQaBk %} </br>
[Building Simple Network Topologies p2](https://www.youtube.com/watch?v=dZKz1J1ZZRU&list=PLKa5I5RRha79V6FNCt2wghrbZaqkU1670&index=1) </br>
{% youtube dZKz1J1ZZRU&list=PLKa5I5RRha79V6FNCt2wghrbZaqkU1670&index=1 %}
</br>

##Main
------------------7月8日更新-----------------------------------
我拿着这个bug在网上半天找不到答案，突然找到一个Google Group里面的回答，发现跟我之前装ns3的方式不一样。于是在之前ns3的兄弟目录装了一个他们说的那种版本的ns3.
http://mailman.isi.edu/pipermail/ns-bugs/2015-February/011975.html</br>
并按照这里面的内容改了一下几个文件(其实就是在一个.cc文件里面include了某个头文件，而在另一个文件.h里取消了include那个头文件，然后改了一下wscript)。
![这里写图片描述](http://img.blog.csdn.net/20160708161817935)
编译这两千个.cc文件也是不简单给ubuntu的2.2G内存用完了，结果还碰到一个g++编译器的问题，得用swap，于是在本来就不多的硬盘上给了1G用作swap，`sudo dd if=/dev/zero of=/swapfile bs=64M count=16` ，`sudo mkswap /swapfile
sudo swapon /swapfile` 。编译完成之后删除`sudo swapoff /swapfile
sudo rm /swapfile`。其实就是在『bindings/ns3module.cc』这个文件编译的时候超级吃内存(感觉得要2G)，这种文件编译完了立马内存就释放了。
http://skypacer210.github.io/2014/12/26/building-ns-3-on-ubuntu/
![这里写图片描述](http://img.blog.csdn.net/20160708161313568)
![这里写图片描述](http://img.blog.csdn.net/20160708161329148)</br>
OK,在这种下载ns3的方式下，编译成功了。</br>
![这里写图片描述](http://img.blog.csdn.net/20160708163046328)
-------------------------我是分割线--------------------------------
最近准备建一些混杂的拓扑(有WIFI，有点对点，有CSMA以太网)，但是OpenNet这个库是刚出来的，有一些小bug，不太好用，于是跟Gilani交流后，他说ns-3可以做，于是我上网找了一些视频开始学习。
看视频里面的代码的一个示例：tutorial4.cc </br>
![这里写图片描述](http://img.blog.csdn.net/20160703030536433)
这里是要建立的拓扑：</br>
![这里写图片描述](http://img.blog.csdn.net/20160703030709762)
实际上每个节点(由`NodeDeviceContainer`建立的)是按照创建顺序然后依次排入一个`NodeList`中，用来为没个节点建立数字索引。</br>
![这里写图片描述](http://img.blog.csdn.net/20160703030854602)
每个网卡(API里面叫`NetDevice`，比如回环网卡`LoopbackNetDevice`, 点对点网卡`PointToPointNetDevice`等)
</br>
![这里写图片描述](http://img.blog.csdn.net/20160703031135826)
一个物理的网络设备可以有安装多个网卡</br>
![这里写图片描述](http://img.blog.csdn.net/20160703031336717)
