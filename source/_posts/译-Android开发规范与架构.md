---
title: '[译]Android开发规范与架构'
date: 2016-07-11 03:25:29
tags: Android
---

## 摘要

 -  使用Gradle和它推荐的工程目录
 -  使用 Maven依赖而不是导入jar包
 -  把密码和敏感数据放在`gradle.properties`
 -  使用`Fragment`来呈现UI视图
 -  使用`Activity`只是用来管理`Fragment`的
 -  使用多个`style`文件以避免单一的大style文件
 -  使用`WebView`时避免在客户端做处理HTML工作，当心内存泄露
 -  总是用ProGuard和DexGuard来混淆项目
 -  不要自己写HTTP客户端，使用Volley或者OKHttp库

## Android SDK存放路径
将你的Android SDK放在你的home目录或其他应用程序无关的位置。当安装有些包含SDK的IDE的时候，可能会将SDK放在IDE同一目录下，当你需要升级（或重新安装）IDE或更换的IDE时，会非常麻烦。
## 构建系统
你的默认编译环境应该是Gradle。因为Ant 有很多限制，也很冗余。[使用Gradle](http://tools.android.com/tech-docs/new-build-system/user-guide)，**管理和下载依赖很方便**。
## 工程结构
不要用老的`Ant& Eclipse ADT`工程结构：
├─ assets
├─ libs
├─ res
├─ src
│  └─ com/futurice/project
├─ AndroidManifest.xml
├─ build.gradle
├─ project.properties
└─ proguard-rules.pro

而用新的`Gradle& Android Studio`工程结构：
├─ library-foobar
├─ app
│  ├─ libs
│  ├─ src
│  │  ├─ androidTest
│  │  │  └─ java
│  │  │     └─ com/futurice/project
│  │  └─ main
│  │     ├─ java
│  │     │  └─ com/futurice/project
│  │     ├─ res
│  │     └─ AndroidManifest.xml
│  ├─ build.gradle
│  └─ proguard-rules.pro
├─ build.gradle
└─ settings.gradle

你的项目引用第三方项目库时（例如，library-foobar），拥有一个顶级包名app从第三方库项目区分你的应用程序是非常有用的。

下载jar包，然后更新它们是很繁琐的，这个问题Maven很好的解决了，这在Android Gradle构建中也是推荐的方法。你可以指定版本的一个范围，如2.1.+,然后Maven会自动升级到制定的最新版本，例如：

```
dependencies {
    compile 'com.netflix.rxjava:rxjava-core:0.19.+'
    compile 'com.netflix.rxjava:rxjava-android:0.19.+'
    compile 'com.fasterxml.jackson.core:jackson-databind:2.4.+'
    compile 'com.fasterxml.jackson.core:jackson-core:2.4.+'
    compile 'com.fasterxml.jackson.core:jackson-annotations:2.4.+'
    compile 'com.squareup.okhttp:okhttp:2.0.+'
    compile 'com.squareup.okhttp:okhttp-urlconnection:2.0.+'
}
```

## Activities与Fragments
Fragments应该作为你实现UI界面默认选择。你可以重复使用Fragments来组合成你的应用。我们强烈推荐使用Fragments而不是Activity来呈现UI界面，理由如下：

-  **屏幕间数据通信**  从一个Activity发送复杂数据(例如Java对象)到另外一个Activity时，Android的API并没有提供合适的方法。不过使用Fragment，你可以使用一个Activity实例作为这个Activity的子Fragments之间的通信通道。这样比Activity与Activity间的通信好。
-  **提供多窗格布局解决方案**  Fragments 的引入主要是为了将手机应用延伸到平板电脑，所以在平板电脑上你可能有A、B两个窗格，但是在手机应用上A、B可能分别充满整个屏幕。如果你的应用在最初就使用了Fragments，那么以后将你的应用适配到其他不同尺寸屏幕就会非常简单。
-  Fragments 一般通用的不只有UI 你可以有一个没有界面的fragment作为Activity提供后台工作。进一步你可以使用这个特性来创建一个[fragment 包含改变其它fragment的逻辑](http://stackoverflow.com/questions/12363790/how-many-activities-vs-fragments/12528434#12528434)，而不是把这个逻辑放在Activity中。

## Java 包结构
Android 应用程序在架构上大致是Java中的Model-View-Controller结构。在Android 中 Fragment和Activity通常上是控制器类(http://www.informit.com/articles/article.aspx?p=2126865).换句话说，他们是用户接口的部分，同样也是Views视图的部分。
正是因为如此，才很难严格的将fragments (或者 activities) 严格的划分成 控制器controlloers还是视图 views。最还是将它们放在自己单独的 fragments 包中。只要你遵循之前提到的建议，Activities 则可以放在顶级目录下。如果你规划有2到3个以上的activity，那么还是同样新建一个activities包吧。
然而，这种架构可以看做是另一种形式的MVC，包含要被解析API响应的JSON数据，来填充的POJO的models包中。和一个views包来包含你的自定义视图、通知、导航视图，widgets等等。适配器Adapter是在数据和视图之间。然而他们通常需要通过getView()方法来导出一些视图，所以你可以将adapters包放在views包里面。
一些控制器角色的类是应用程序级别的，同时是接近系统的。这些类放在managers包下面。一些繁杂的数据处理类，比如说"DateUtils",放在utils包下面。与后端交互负责网络处理类，放在network包下面。

总而言之，以最接近用户而不是最接近后端去安排他们。
com.futurice.project
├─ network
├─ models
├─ managers
├─ utils
├─ fragments
└─ views
   ├─ adapters
   ├─ actionbar
   ├─ widgets
   └─ notifications

## 资源文件命名
遵循`前缀表明类型`的习惯，形如type_foo_bar.xml。例如：`fragment_contact_details.xml`,</br>`view_primary_button.xml`</br>,`activity_main.xml`</br>.

## 组织布局文件
若果你不确定如何排版一个布局文件，遵循以下下规则可能会有帮助。

- 每一个属性一行，缩进4个空格
- android:id 总是作为第一个属性
- android:layout_**** 属性在上边
- style 属性在底部
- 关闭标签/>单独起一行，有助于调整和添加新的属性。

例如：
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <TextView
        android:id="@+id/name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:text="@string/name"
        style="@style/FancyText"
        />

    <include layout="@layout/reusable_part" />

</LinearLayout>
```

## 将一个大的style文件分割成多个文件
你可以有多个styles.xml 文件。Android SDK支持其它文件，styles这个文件名称并没有作用，起作用的是在文件里xml的style标签。因此你可以有多个style文件。styles.xml,style_home.xml,style_item_details.xml,styles_forms.xml。不用于资源文件路径需要为系统构建起的有意义，在res/values目录下的文件可以任意命名。
## 关于colors.xml
colors.xml是一个调色板 在你的colors.xml文件中应该只是映射颜色的名称一个RGBA值，而没有其它的。不要使用它为不同的按钮来定义RGBA值。
不要这样做
```xml
<resources>
    <color name="button_foreground">#FFFFFF</color>
    <color name="button_background">#2A91BD</color>
    <color name="comment_background_inactive">#5F5F5F</color>
    <color name="comment_background_active">#939393</color>
    <color name="comment_foreground">#FFFFFF</color>
    <color name="comment_foreground_important">#FF9D2F</color>
    ...
    <color name="comment_shadow">#323232</color>
</resources>
```
使用这种格式，你会非常容易的开始重复定义RGBA值，这使如果需要改变基本色变的很复杂。同时，这些定义是跟一些环境关联起来的，如button或者comment,应该放到一个按钮风格中，而不是在color.xml文件中。


相反，这样做:
```xml
<resources>

    <!-- grayscale -->
    <color name="white"     >#FFFFFF</color>
    <color name="gray_light">#DBDBDB</color>
    <color name="gray"      >#939393</color>
    <color name="gray_dark" >#5F5F5F</color>
    <color name="black"     >#323232</color>

    <!-- basic colors -->
    <color name="green">#27D34D</color>
    <color name="blue">#2A91BD</color>
    <color name="orange">#FF9D2F</color>
    <color name="red">#FF432F</color>

</resources>
```
**像这样规范的颜色很容易修改或重构，会使应用一共使用了多少种不同的颜色变得非常清晰。**

## 小心关于WebViews的问题
如果你必须显示一个web视图，比如说对于一个新闻文章，避免做客户端处理HTML的工作，最好让后端工程师协助，让他返回一个 "纯" HTML。WebViews 也能导致内存泄露当保持引他们的Activity，而不是被绑定到ApplicationContext中的时候。当使用简单的文字或按钮时，避免使用WebView，这时使用TextView或Buttons更好。
// TODO 2016-3-2 2:57
参考文章：http://www.jianshu.com/p/4390f4fe19b3
http://www.jianshu.com/p/d9e4ddd1c530
