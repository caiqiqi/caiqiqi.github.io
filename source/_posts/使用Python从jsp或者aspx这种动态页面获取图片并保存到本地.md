---
title: 使用Python从jsp或者aspx这种动态页面获取图片并保存到本地
date: 2016-07-11 03:27:14
tags: [Python, Web]
---

有些登录页面中的验证码图片是从这样的动态页面url：</br>
http://cer.nju.edu.cn/amserver/verify/image.jsp</br>
http://gs.cqupt.edu.cn:8080/Public/ValidateCode.aspx </br>
中获取的。这时不能这样：
``` python 
def save_img_from_url(imageUrl, filename):
	u = urllib2.urlopen(imageUrl)
	data = u.read()
	f = open(filename, 'wb')
	f.write(data)
	f.close()
```
因为得到的是一个html页面，查看其源代码如下：
![这里写图片描述](http://img.blog.csdn.net/20160311194509843)
而应该用：
哦不是，上面那样可以。。。。
