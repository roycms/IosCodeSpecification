# 图片命名规则

>引用自简书 offspring  http://www.jianshu.com/p/2896b2823b65

为了降低设计和开发之间的交流成本，所以在此确定一下 iOS 项目切图的命名标准。

我们的命名规则的基本思想是把文件名分成三部分，第一部分是图片的逻辑归属分类，第二部分是图片的表现内容，第三部分是图片的内容的类型，有些图片还会有第四部分，表示图片表现的状态。

首先有几个规则是：

* 用英文命名，不用拼音
* 每一部分用下划线分隔
* 图片名中两倍图在名字最后要加@2x，三倍图在名字最后要加@3x

## 逻辑分类

逻辑分类即是这张图片所属的分组，在 iOS 中大多的项目是以 Tab bar 的形式进行逻辑上的分组，以下图为例：

 ![cup_image.png](https://roycms.github.io/IosCodeSpecification/cup_image.png)
 
这张图中的 Showtime 客户端就是使用 Tab bar 进行分组的，可以看出内容分成了五个部分：Home，Categories，Live TV，My List 和 Search。

所以例如在这个界面中，右上角的设置按钮，那么它的切图命名的 第一部分就是 Home。

但是仔细看看上面这个界面的话有一些图其实是不属于某个分类的，比如 Tab bar 中的图标文件和 Navigation Bar 中的 Showtime 图标，对于这两种内容来讲，它们命名的规范是第一部分显示的是 navigationbar 或者是 tabbar。

## 表现内容

表现内容就是这个图片表现的内容，同样以上图为例，界面中右上角的按钮表现的是设置按钮，那么它的表现内容就是设置，所以这个按钮在第二部分显示的就是 settings。

而在上图的 Tab bar 中第二部分的名字就是它们在程序中显示的名字 ，而 Navigation Bar 中的图标的第二部分命名因为表现的内容就是 Showtime，所以显示的就是 showtime。


## 内容类型
同样以 Showtime 的图为例，在 Navigation Bar 中的图标，它的内容类型是 icon，所以这个图片文件的两倍图完整命名就是 navigationbar_showtime_icon@2x.png。
而右上角的设置按钮的话，它的类型是按钮，所以第三部分即是 button 的缩写 btn，即是 navigationbar_settings_button，下面五个 Tab bar 的图片名分别是 tabbar_home_icon,tabbar_categories_icon,tabbar_livetv_icon,tabbar_mylist_icon 和 tabbar_search_icon。

## 图片状态
对于某些类型的切图，它可能代表的只是某个控件的一种状态，以按钮为例，正常的状态就是 normal，而点击中的状态是 highlighted，选中的状态则是 selected。
Showtime 的图中，右上角的设置图标是正常的状态，所以加入图片的状态之后，它的两倍图的完整命名应该是 navigationbar_settings_btn_normal@2x.png。
而下方的 Tab bar 来讲，选中的是 Home，那么当前这张图片的两倍图的完整命名就应该是 tabbar_home_icon_selected@2x.png。
