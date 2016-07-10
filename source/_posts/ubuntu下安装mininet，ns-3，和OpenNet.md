---
title: ubuntu下安装mininet，ns-3，和OpenNet
date: 2016-07-11 01:47:38
tags: [SDN, linux]
---

## Mininet
首先得有mininet的系统环境，到mininet的官方GitHub主页上说的 http://mininet.org/download/ 
> The easiest way to get started is to download a pre-packaged Mininet/Ubuntu VM. This VM includes Mininet itself, all OpenFlow binaries and tools pre-installed, and tweaks to the kernel configuration to support larger Mininet networks.

虚拟机文件地址</br> 
https://github.com/mininet/mininet/wiki/Mininet-VM-Images </br>
我选的是这个版本的</br> 
http://downloads.mininet.org/mininet-2.2.1-150420-ubuntu-14.04-server-amd64.zip </br>
然后在VMware player(Windows) 或者VMware Fusion(OSX)里运行。
系统里设置好了默认用户是`mininet`，密码也是`mininet`。</br>
Tips：可以改一下root的密码`sudo passwd root`。
这里提一下如果根目录太小了，可以清理一下apt-get的默认下载路径`/var/cache/apt/archives`里面的已经安装过的.deb文件。
## Ubuntu server版本图形界面
默认ubuntu server是没有图形界面的，有这样的理由：
1 有更过依赖的服务，更有可能down机，也引入了更多的缺陷。
2 资源(CPU，硬盘，内存)会耗费。
可以在ubuntu上安装 X11服务端</br>
`sudo apt-get install xorg`
`sudo apt-get install openbox`
然后在宿主机上安装X11客户端</br>
`sudo apt-get install xauth`</br>
附链接：</br>
[Displaying Ubuntu Linux Applications Remotely (X11 Forwarding)](http://www.techotopia.com/index.php/Displaying_Ubuntu_Linux_Applications_Remotely_(X11_Forwarding))

## ns-3
官方提供的安装姿势：</br>
链接是 https://www.nsnam.org/wiki/Installation </br>
安装ns-3之前有很多依赖包要安装，这里根据官网的wiki列出来一下
``` bash
sudo apt-get install gcc g++ python python-dev qt4-dev-tools libqt4-dev mercurial bzr cmake libc6-dev libc6-dev-i386 g++-multilib gdb valgrind gsl-bin libgsl0-dev libgsl0ldbl flex bison libfl-dev sqlite sqlite3 libsqlite3-dev libxml2 libxml2-dev libgtk2.0-0 libgtk2.0-dev vtun lxc uncrustify doxygen graphviz imagemagick texlive texlive-extra-utils texlive-latex-extra texlive-font-utils texlive-lang-portuguese dvipng python-sphinx dia python-pygraphviz python-kiwi python-pygoocanvas libgoocanvas-dev libboost-signals-dev libboost-filesystem-dev openmpi-bin openmpi-common openmpi-doc libopenmpi-dev
``` 
</br>


参考：</br>
http://blog.csdn.net/roger__king/article/details/9272141
http://skypacer210.github.io/2014/12/26/building-ns-3-on-ubuntu/
http://mailman.isi.edu/pipermail/ns-bugs/2015-February/011975.html


