---
title: 研究生管理系统前端代码和HTTP通信过程分析
date: 2016-07-11 03:20:12
tags: [HTTP, web]
---

##主页面
http://gs.cqupt.edu.cn:8080/Gstudent/Default.aspx
![这里写图片描述](http://img.blog.csdn.net/20160309165759223)
### 在浏览器中输入...Default.aspx之后
从Chrome的调试工具中可以看到，先加载default.aspx，然后如果之前有缓存过的js,css,图片等资源时，就从缓存中读取这些缓存的资源，
![这里写图片描述](http://img.blog.csdn.net/20160312114722380)
注意Referer都是 http://gs.cqupt.edu.cn:8080/gstudent/default.aspx
![这里写图片描述](http://img.blog.csdn.net/20160312115014585)
##登录页面
http://gs.cqupt.edu.cn:8080/Gstudent/Relogin.aspx
或者
http://gs.cqupt.edu.cn:8080/gstudent/ReLogin.aspx?ReturnUrl=/Gstudent/loging.aspx
![这里写图片描述](http://img.blog.csdn.net/20160309165927895)

发现在：</br>
第一次载入`Relogin.aspx`页面的时候，request里面是**没有sessionId的**。而在response里Set-Cookie里面设置里cookie的SeesionId字段。</br>
![这里写图片描述](http://img.blog.csdn.net/20160311212300107)
然后在</br>
第二次请求的时候，request中的cookie会带上这个SeesionId字段，保存着之前的SessionId值</br>
![这里写图片描述](http://img.blog.csdn.net/20160311212150871)
由此想到可以请求一次（带上随便的几个参数然后post上去就得意获取最新生成的SessionId）

## 退出按钮
```
<a id="DataList1_ctl05_hykMenu" class="none" onclick="if(confirm('确定要退出系统吗?')){parent.location.href ='../UserLogin.aspx?exit=1'}" href="javascript:void(0)">退出</a>
```
## 课程查询页面
`gs.cqupt.edu.cn:8080/Gstudent/Course/CourseSelFull.aspx`
先找`登录`按钮
登录按钮对应的`span`标签：
```html
<span id="ctl00_contentParent_UpdatePanel2"> <a class="l-btn" id="btLogin" tabindex="4" title="登录系统" href='javascript:WebForm_DoPostBackWithOptions(new%20WebForm_PostBackOptions("ctl00$contentParent$btLogin",%20"",%20true,%20"",%20"",%20false,%20true))'><span class="l-btn-left"><span class="l-btn-text">登录</span></span></a></span>
```
即，其id为`btLogin`
```html
<form id="Form1" name="Form1" method="post" action="./ReLogin.aspx" 
    onsubmit="javascript:return WebForm_OnSubmit();" 
    onkeypress="javascript:return WebForm_FireDefaultButton(event, 'btLogin')">
```
关于`WebForm_OnSubmit`
```
<script type="text/javascript">
//<![CDATA[
function WebForm_OnSubmit() {
if (typeof(ValidatorOnSubmit) == "function" && ValidatorOnSubmit() == false) return false;
return true;
}
//]]>
</script>

function ValidatorOnSubmit() {
    if (Page_ValidationActive) {
        return ValidatorCommonOnSubmit();
    }
    else {
        return true;
    }
}
```
请求到时候会包含这几个脚本：
```
<script src="/WebResource.axd?d=5d5KGRMAyLvBqg6D0Z6Xhj3Bik0iH8Qt5h7ZqLwbqRWMcG_QNGx9uHHYTzhIEQhPxWH_P8u88WPt72kX0&amp;t=635793306349294682" type="text/javascript"></script>


<script src="/ScriptResource.axd?d=AckwrukOP9WZOQlm3XXq4YcrzYbdSQ2KhWkywbSm1aGoDT_FKtbRYITwBp3NhSjhrizBahjqDw84LDkin5GAuA35K9SQhCDJ08MfQmfH_dkIfL6Ri2ppB1doY17oRpPfe_KiLMv-Dc3ioc5o0&amp;t=fffffffffa488f4d" type="text/javascript"></script>
<script src="/ScriptResource.axd?d=a8XuVJe3RPmNNezBCV4fnhmFCTQPZUJuIVB1PICLRIFB0h0iFOT51is_Q9pzVzBg5YWBLBKS1CWtbSPylPaRTjWmvnSuN_4Vp_BgSDtPqcL64afEn05pWbSSNySR9Mydpi-0VtLHKkLXQpp5CBE1Sm7fImxVdYLGKHFTJg2&amp;t=72e85ccd" type="text/javascript"></script>
```
```html
<script type="text/javascript">
//<![CDATA[
var theForm = document.forms['Form1'];
if (!theForm) {
    theForm = document.Form1;
}
//前台调用后台aspx的代码
function __doPostBack(eventTarget, eventArgument) {
    if (!theForm.onsubmit || (theForm.onsubmit() != false)) {
        theForm.__EVENTTARGET.value = eventTarget;
        theForm.__EVENTARGUMENT.value = eventArgument;
        theForm.submit();
    }
}
//]]>
</script>
```
注：`document.forms['formname']`得到以`formname`为名的表单
initForm()函数定义：
```html
<!--initForm() 函数定义-->
    <script type="text/javascript">
        function initForm() {
            $('#btLogin').linkbutton();
        }      
    </script>
```
注：`$('#btlogin')`的意思是通过jQuery找到 id 为`btLogin`的元素。然后`.linkbutton()`这里是使用了![EasyUI](http://www.jeasyui.com/documentation/linkbutton.php)这个框架的API。其作用是创建一个超链接按钮，表示普通的a标签。按钮可为图标＋文本或图标或文本。宽度可修改。关于`$("#btn").linkbutton()`的作用，详见![这篇文章](http://www.cnblogs.com/haogj/archive/2013/05/13/3074461.html)
即，可以把那个按钮变成特殊的效果，和加上`class ＝ easyui-linkbutton` 的作用是一样的。
Create a linkbutton from markup is more easily. 

`
 <a id="btn" href="#" class="easyui-linkbutton" data-options="iconCls:'icon-search'">easyui</a>
 `
Create a linkbutton programmatically is also allowed. 

如下：

```
    <a id="btn" href="#">easyui</a>
```
然后：
```
    $('#btn').linkbutton({
        iconCls: 'icon-search'
    });
```

```html
<script type="text/javascript">      
         $(document).ready(function () {if(typeof (initForm)=="function"){ initForm()}})
         $(document).ready(function () { if (typeof (reload) == "function") { reload() } })   
    </script>
```
注：
-  js中`typeof` 可以用来检测给定变量的数据类型。
-  `$(document).ready(function(){...})` 表示页面加载之后开始运行。

从上面可以看出，过程如下：
页面加载之后，执行`initForm()`,然后是`reload()`

关于几个隐藏的 input
```
<div>
<input name="__EVENTTARGET" id="__EVENTTARGET" value="" type="hidden">
<input name="__EVENTARGUMENT" id="__EVENTARGUMENT" value="" type="hidden">
<input name="__VIEWSTATE" id="__VIEWSTATE" value="/wEPDwUKMTA4ODc5NDc0OA9kFgJmD2QWAgIDD2QWAgIDD2QWAgILD2QWAmYPZBYCAgEPDxYCHghJbWFnZVVybAUrfi9QdWJsaWMvVmFsaWRhdGVDb2RlLmFzcHg/aW1hZ2U9MTEyNjY2NzY2OGRkGAEFHl9fQ29udHJvbHNSZXF1aXJlUG9zdEJhY2tLZXlfXxYBBSFjdGwwMCRjb250ZW50UGFyZW50JFZhbGlkYXRlSW1hZ2U=" type="hidden">
</div>

```
等前面都加载完之后，再在script里面加载三个aspx页面。
![这里写图片描述](http://img.blog.csdn.net/20160313230126150)
于是有了后面的三个请求，然而这里的`uid`未被赋值，所以几个请求都是`.aspx?undefined`
