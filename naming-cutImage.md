# ios切图命名规范

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

# IOS切图规则

我们都知道一套完整的 App 通常会有很多张切图，不管是 iPhone 需要 1x、2x、3x 图档，Android 需要至少 3 种 hdpi、xhdpi、xxhdpi。

在庞大的数量下如何让负责套图的 RD 快速找到所需图档，档名的命令方式就需要双方统一格式方便大家作业。

所以，制定一套非常有效而方便的APP切图命名规范非常有用的。

目前iPhone有10种型号，5种屏幕尺寸，再加上6plus的“降采样”（Downsampling）（1080-1920），还有iPhone6和6+上的放大模式（1125-2001）和默认模式（1242-2208），是不是感觉好恐怖？但是不用怕，我分享一套超简单的适配方法，看完你都不信有这么简单~

美术交付给开发的资料有

* 标注图（以640为宽度尺寸为基准标注）

* 2x切图（以640为宽度尺寸为基准切图）

* 3x切图（以1280为宽度尺寸为基准切图）

 ![cup_image.png](https://roycms.github.io/IosCodeSpecification/iphone.png)

开发看到这份标注图，可以自己用上面的数字，乘以1.5得出3X的数字。

### 为什么3x切图要以1280来为宽度？

其实iPhone6+的尺寸1242*2208作为3X，怎么算都又难记又不能整除，我们直接640*2得到1280跟1242相差也没几十个像素，最重要的是不虚边啊，放在真机上看（处女座除外）看不出差别的。

### 为什么只设宽度？

为了保持长宽比例。iPhone的这几个尺寸不是正好的，宽度放大后高度总差那么几个像素，这不要紧，千万别去改高度，手机屏幕是可以上下滑动的嘛。
不可以滑动必须保证一屏显示的除外，手动去调整好了。

### 为什么开发不是乘以2而是乘以1.5来算尺寸和字号？因为大屏手机就是要显示更多内容而存在的。纯等比放大界面看起来傻大傻大的，实验证明1.5倍是正好的。


# iOS切图脚本文件

大家将以下代码复制下来再Xcode中，建一个以.sh结尾的文件，然后复制出来再一个自己的文件夹中。

icon的基准的图片尺寸为1024*1024。

接下来你只需要将该文件夹直接拖到命令行就好了。
```
#! /bin/bash

# prepare
ROOT_DIR=$(pwd)

#check file exist
SOURCE_FILE="${ROOT_DIR}/1024.png"
echo $SOURCE_FILE
if [[ ! -e ${SOURCE_FILE} ]]; then
    echo "文件不存在"
    exit 1
fi
DEST_DIR="${ROOT_DIR}/icon"
#如果目录有图片先清空
if [[ -d ${DEST_DIR} ]]; then
    rm -rf dir ${DEST_DIR}
fi
mkdir -p "${DEST_DIR}"
Image_NAME=("29.png" "29@2x.png" "40.png" "40@2x.png" "87.png" "57.png" "57@2x.png" "76.png" "76@2x.png" "60@2x.png" "60@3x.png")
Image_SIZE=("29" "58" "40" "80" "87" "57" "114" "76" "152" "120" "180")


#sips starting
cp "${SOURCE_FILE}" "${DEST_DIR}"
for ((i=0; i<${#Image_SIZE[@]} ;i++)); do
    size=${Image_SIZE[i]}
    sips -Z ${size} "${SOURCE_FILE}" --out "${DEST_DIR}/${Image_NAME[i]}"
done

```
