# Xcode 工程规范







如果可以的话，尽可能一直打开 target Build Settings 中 "Treat Warnings as Errors" 以及一些[额外的警告][Xcode-project_1]。如果你需要忽略指定的警告,使用 [Clang 的编译特性][Xcode-project_2] 。


[Xcode-project_1]:http://boredzo.org/blog/archives/2009-11-07/warnings
[Xcode-project_2]:http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas

## 目录结构参考
* 为了避免文件杂乱，物理文件应该保持和 Xcode 项目文件同步。
* Xcode 创建的任何组（group）都必须在文件系统有相应的映射。
* 为了更清晰，代码不仅应该按照类型进行分组，也可以根据功能进行分组。
```
projectName/
    Resources                   图片等素材
    Sources                     代码
        /ViewController         控制器
        /Service                一些第三方比如支付宝，提供给controller的封装好的接口
        /External               引入的第三方库
        /Net                    网络请求
        /Model                  模型
        /Util                   工具类
        /Custom                 自定义的文件
            /Catergory          自定义的扩展类
            /Config             项目配置文件
            /View               视图
            /Util               工具类
```

## 编译警告
* 推荐你尽可能多打开编译警告，并且像对待错误一样对待编译警告。推荐 这个PPT。
* 这个幻灯片覆盖了如何在特定文件，或者特别代码段里面消除相关警告的内容。
简单的来说，至少需要在 _“Other Warning Flags” 编译设置里面定义下面的值：
```
-Wall _(增加很多的警告)_
-Wextra _(增加更多的警告)_
同时打开 “Treat warnings as errors”
```

## Clang 静态分析

* Clang 编译器（Xcode使用的）有一个 静态分析器 来进行你的代码控制和数据流的分析，来检测编译器不能检测的许多错误。
* 你可以通过在 Xcode 里面手动运行 Product → Analyze 菜单项来手动执行代码分析
* 分析器可以用浅或者深的模式允许，后者更加慢，但是可以从跨函数的控制流和数据流上分析更多问题
** 推荐 **

* 打开 所有 分析器检查 (通过在 building setting 中打开所有 “Static Analyzer” 选项)
* 在 release 的编译设置里面打开 “Analyze during ‘Build’” 来让分析器自动在发布的版本构建的时候允许。(这样你就不需要记住要手动运行了)
* 把 “Mode of Analysis for ‘Analyze’” 设置为 Shallow (faster)
* 把 “Mode of Analysis for ‘Build’” 设置为 to Deep

## Faux Pas

* 我们自己的Ali Rantakari 创建的，Faux Pas 是一个极佳的静态错误检测工具，它分析你的代码并且找出那些你自己甚至都没发现的问题。在提交你的 App 到应用商店前用它吧！

## Assets 资源

* Asset catalogs 是管理你所有项目可视化资源的最好方式，它们可以同事管理通用的以及设备相关的iPhone 4-inch, iPhone Retina, iPad,等)资源，并且会自动通过它们的名字分组。
* 告诉你的设计师如何添加它们（Xcode有内置的 Git 客户端）可以节省很多时间，否则你会话很多时间来在从邮件或者其他渠道复制到代码库里面。它可以让设计师马上尝试资源改变的样子，并且反复实验。

## Using Bitmap Images 使用位图

* Asset catalogs 仅仅暴露了图片的名称，图片集里面的抽象的名字。这可以避免资源名字的冲突，就像 button_large@2x.png 的文件的命名空间在它的图片集里面。遵守一些命名规则可以让生活更美好：
```
IconCheckmarkHighlighted.png // Universal, non-Retina
IconCheckmarkHighlighted@2x.png // Universal, Retina
IconCheckmarkHighlighted~iphone.png // iPhone, non-Retina
IconCheckmarkHighlighted@2x~iphone.png // iPhone, Retina
IconCheckmarkHighlighted-568h@2x~iphone.png // iPhone, Retina, 4-inch
IconCheckmarkHighlighted~ipad.png // iPad, non-Retina
IconCheckmarkHighlighted@2x~ipad.png // iPad, Retina
```
修饰后缀 -568h, @2x, ~iphone and ~ipad 是不必要的，但是有他们在文件里面，当把文件拖进去的时候，Xcode会正确地处置它们。这避免赋值错误。

## 使用向量图

* 你可以用设计师原始的 vector graphics (PDFs) 加入到 asset catalogs，Xcode可以自动地根据它们生成位图。这减少了你的工程的复杂性（管理更少的文件）。
