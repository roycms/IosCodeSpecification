## CocoasPod

### CocoaPods 安装前准备
* 移除Ruby默认源
  * gem sources --remove https://rubygems.org/
* 使用新的源
  * gem sources -a https://ruby.taobao.org/
* 验证新源是否替换成功
  * gem sources -l
* 替换成功后
  *  *** CURRENT SOURCES * * *
  * https://ruby.taobao.org/

### CocoaPods 安装
* sudo gem install cocoapods
  * **注意：苹果系统升级 OS X EL Capitan 后改为：
 sudo gem install -n /usr/local/bin cocoapods (v1.0.0)**
* pod setup(//将 CocoaPods Specs repository复制到你电脑上~/.cocoapods目录下)
* sudo gem update --system

### CocoaPods 的使用
* 如果你计划增加外部依赖(比如，第三方库)在你的项目中，CocoaPods 提供了一个快捷的途径.
* 新建工程，并在终端中cd到工程文件夹内,在你的 iOS 项目目录下运行：
```
pod init
```
* 它会创建一个 Podfile， 会管理你所有的依赖，在 Podfile 中加入你的依赖
* 编辑文件：platform:ios, '6.0'   
 pod 'AFNetworking', '~> 2.3.1'    <-------第三方
* 执行下面命令安装 
```
pod install
```
* 来安装第三方库并且将它们作为 workspace 的一部分，你的 workspace 也会包含你自己的项目。 
* 一般推荐提交你自己的项目的依赖，而不是每个开发者在一个 checkout之后运行 pod install 。
* 注意在之后，你需要打开 .xcworkspace 而不是 .xcproject，否则你的代码就不能被编译了，命令：
```
pod update
```
会升级所有的 pod 到最新版本，你可以用大量 符号 来定义你期望的版本需求。

## 将代码提交到CocoaPods
*如果你在github共享了一段代码，你想把代码提交到CocoaPods，希望其他用户search 你的项目的名称就可以找到你，并且 pod install 集成你的代码到项目内，你就需要学会怎样将自己的代码先提交给 CocoaPods servers.
 >http://www.cnblogs.com/wengzilin/p/4742530.html
