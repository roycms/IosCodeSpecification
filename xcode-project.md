# Xcode 工程规范

## 目录结构参考
* 为了避免文件杂乱，物理文件应该保持和 Xcode 项目文件同步。
* Xcode 创建的任何组（group）都必须在文件系统有相应的映射。
* 为了更清晰，代码不仅应该按照类型进行分组，也可以根据功能进行分组。

方案一（推荐）
```
projectName/
         ├─ Resources               文件等素材
         ├─ projec           
           ├─ module1               功能模块1
             ├─ model               模型
             ├─ Controllers         控制器
             ├─ Views               视图
           ├─ module2               功能模块2
             ├─ model               模型
             ├─ Controllers         控制器
             ├─ Views               视图
         ├─ Helpers                 工具类
         ├─ Net                     网络请求
         ├─ Config                  项目配置为文件
         ├─ Catergory               自定义扩展类
         ├─ Manager                 管理类
         

```
方案二
```
projectName/
         ├─ Resources               文件等素材
         ├─ Models                  模型
            ├─ module1              功能模块1
            ├─ module2              功能模块2
         ├─ Views                   视图
            ├─ module1              功能模块1
            ├─ module2              功能模块2
         ├─ Controllers             控制器
            ├─ module1              功能模块1
            ├─ module2              功能模块2
         ├─ Helpers                 工具类
         ├─ Net                     网络请求
         ├─ Config                  项目配置为文件
         ├─ Catergory               自定义扩展类
         ├─ Manager                 管理类
         

```

## 编译警告
* 如果可以的话，尽可能一直打开 target Build Settings 中 "Treat Warnings as Errors" 以及一些[额外的警告][Xcode-project_1]。
* 如果你需要忽略指定的警告,使用 [Clang 的编译特性][Xcode-project_2] 。
[Xcode-project_1]:http://boredzo.org/blog/archives/2009-11-07/warnings
[Xcode-project_2]:http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas
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

## 调试
* 当你的 App 崩溃的时候，Xcode 不会默认进入到调试器里面。
* 为了调试，你需要增加一个异常断点（在 Xcode 的 Debug 导航中点 “+”），来在异常发生的时候退出执行。在很多情况下，你需要看看触发这些异常的代码。
* 它会捕捉任何异常，即使是已经处理的。如果 Xcode 在 一个第三方库里面中断执行，比如，你可能需要通过选择 Edit Breakpoint 并且设置 Exception 为 Objective-C.。
* 对于视图 debug， Reveal 和 Spark Inspector 这两个强有力的可视化检查工具可以帮你省下很多时间，特别是在你使用 Auto Layout 并且希望定位出问题或者溢出屏幕的视图的时候。Xcode 提供了免费的类似功能 ，但是只能适用于 iOS 8+ 并且不那么好用。

## 分析
* Xcode 有一个叫 Instruments 的分析工具，它包括了许多分析内存，CPU，网络通讯，图形以及更多的工具，它有点复杂的，但是它的追踪内存泄漏的时候还是蛮直观的。
* 只需要在 Xcode 中 选择 Product > Profile，选择 Allocations， 点击 Record 按钮并且用一些有用的字符串过滤申请空间的信息，比如你自己的app的类名。* 它会在固定的列中统计，并且告诉你每个对象有多少实例。到底是什么类一直增加实例导致内存泄漏。
* Instruments 也有自动化的工具来进行录制并且运行UI交互以及JavaScript文件。
* . UI Auto Monkey 是一个自动化随机点击、滑动以及旋转你的app的脚本，他在压力、渗透测试中很有用。

## Crash Logs 崩溃日志

应该让你的 app 向一个服务发送崩溃日志。你可以手动实现，通过 PLCrashReporter 以及你自己的后端。但是强烈推荐你使用现有的服务，比如下面的

* [Crashlytics](http://www.crashlytics.com/)
* [HockeyApp](http://hockeyapp.net/)
* [Crittercism](https://www.crittercism.com/)
* [Splunk MINTexpress](https://mint.splunk.com/)

当你配置好后，确保你 保存了 the Xcode archive (.xcarchive) 对于每一个 app 放出的版本。

这个 归档中包含了构建的app的二进制以及调试符号(dSYM)，你需要用每个版本特定的app把你的 Crash 报告符号化。
