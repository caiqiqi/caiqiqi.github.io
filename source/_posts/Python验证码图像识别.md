---
title: Python验证码图像识别
date: 2016-07-11 03:31:41
tags: [Python, 验证码]
---

今天又到了我们学校一年一度的选课时间，然后又到了特别想写一个自动登录的东西，但是关键在于验证码如何识别。今天跟老师交流了一下，老师要我可以跟一个大神交流。大神说什么语言都可以实现这个，网上有很多这种可以识别验证码的程序，然后我搜了一下，搜到这个项目。
看到[这个项目](https://github.com/JoveYu/code-identify/blob/master/code.py)里面，用到了一句：
```python
from PIL import Image
```
于是我搜索
```shell
pip install PIL
```
提示没有安装`pip`
于是我先得安装pip。
直接
```
easy_install pip
```
提示没有权限。
```shell
sudo easy_install pip
```
之后，成功安装pip。
![这里写图片描述](http://img.blog.csdn.net/20160307205545588)
然后就可以用批判来安装python模块了。
然后我以为那个
```python
from PIL import Image
```
的模块名叫PIL，于是我
```shell
pip install PIL
```
结果不行，然后看到![这里](https://pypi.python.org/pypi/Pillow/2.1.0)告诉我应该
```shell
pip install Pillow
```
然而还是出现问题，**权限问题！**
![这里写图片描述](http://img.blog.csdn.net/20160307210353588)
于是我
```shell
sudo pip install Pillow
```
就成功了。
待续。。。
