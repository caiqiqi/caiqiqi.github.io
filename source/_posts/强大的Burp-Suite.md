---
title: 强大的Burp Suite
date: 2016-07-11 02:01:49
tags: web
---
Oh，my burp，认识你真是一个曲折的过程。之前用我那个Win8的电脑，由于屏幕太小了，而你的界面上的字又太小，导致对你的体验不太好，让我对你有一些排斥心理，现在好了，有Macbook Pro了，体验绝佳！屏幕大了，CPU快了，内存有保障了，再看你的时候就舒服多了。不过找你的professional的crack版也弄了半天。不过最后总算是把你这个jar文件加到环境变量中去了。然后给你来一个hotkey。</br>
`vi ~/.zshrc`，然后`alias burp="java -jar /Users/caiqiqi/Downloads/burpsuite_pro_v1.5.01_crack/BurpLoader.jar"`
就把你放在~/downloads目录下吧。然后`source ~/.zshrc`让你生效(不过这个生效不对之前打开的终端标签页有用哦，当然新开一个终端标签页是有用的)。然后我就可以在终端用`burp`命令来启动你了哇！然后发现果然这个burp跟以前一直用的chrome或者firefox自带的开发者工具是不一样的。burp更加强大！看这个，他可以拦截每次request和response，并且你都可以修改！
![这里写图片描述](http://img.blog.csdn.net/20160425172453634)</br>
而chrome或者firefox的开发者只是记录这些请求和响应，不能更改。</br>
![这里写图片描述](http://img.blog.csdn.net/20160425173042400)
![这里写图片描述](http://img.blog.csdn.net/20160425173156840)
