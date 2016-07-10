---
title: ' OSX从系统自带python改为Homebrew版python'
date: 2016-07-11 03:28:36
tags: [OSX, Python, Homebrew]
---

```shell
# 将Homebrew版python将要放的路径加入到环境变量
export PATH =/usr/local/bin:$PATH
# 使修改后的环境变量生效
source ~/.bash_profile
# 再判断一下当前的python是哪个python
which python
# 如果显示的是/usr/local/bin/python(Homebrew版)而不是/usr/bin/python(系统自带)，则算成功
```
![这里写图片描述](http://img.blog.csdn.net/20160308225443445)

之前用系统自带python时，
![这里写图片描述](http://img.blog.csdn.net/20160308231835892)
当安装好Homebrew版的python之后，
![这里写图片描述](http://img.blog.csdn.net/20160308232007846)
