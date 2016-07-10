---
title: mininet实战3
date: 2016-07-11 02:13:16
tags: [SDN, mininet]
---

在`/home/cqq/repos/mininet-wifi/examples`目录
`sudo python wifiMobility.py`
PS: 就运行这个脚本，导致ubuntu经常死机，于是我只得给ubuntu加到2G内存，两个核处理器了。
![这里写图片描述](http://img.blog.csdn.net/20160516193441073)
运行之后，发现sta1和sta2之间ping的时候，延迟是先最大，然后从一个小的值慢慢增大，然后稳定到180ms。应该画出图到图上瞧瞧。
![这里写图片描述](http://img.blog.csdn.net/20160516193553917)
PS：原来就这样就可以画图了啊
```python
from mininet.net import Mininet
"Create a network."
et = Mininet( controller=Controller, link=TCLink, switch=OVSKernelSwitch )
net.plotGraph(max_x=60, max_y=60)
```

Edit on 5/18 00:21-------------------------------------------
找到了PACKT_IN 和PACKET_OUT的包
![这里写图片描述](http://img.blog.csdn.net/20160518002235827)
![这里写图片描述](http://img.blog.csdn.net/20160518002256015)
![这里写图片描述](http://img.blog.csdn.net/20160518002330687)
![这里写图片描述](http://img.blog.csdn.net/20160518002346281)
![这里写图片描述](http://img.blog.csdn.net/20160518002403495)

