## CocoasPod的安装、使用以及常见错误处理:
#### CocoaPods 安装前的准备
* 移除Ruby默认源
  * gem sources --remove https://rubygems.org/
* 使用新的源
  * gem sources -a https://ruby.taobao.org/
* 验证新源是否替换成功
  * gem sources -l
* 替换成功后
  *  *** CURRENT SOURCES * * *
  * https://ruby.taobao.org/

#### CocoaPods 的安装
* sudo gem install cocoapods
  * **注意：苹果系统升级 OS X EL Capitan 后改为：
 sudo gem install -n /usr/local/bin cocoapods (v1.0.0)**
* pod setup(//将 CocoaPods Specs repository复制到你电脑上~/.cocoapods目录下)
* sudo gem update --system
* 新建工程，并在终端中cd到工程文件夹内
* pod search 第三方
* 新建文件：vim Podfile
* 编辑文件：platform:ios, '6.0'   
 pod 'AFNetworking', '~> 2.3.1'    <-------第三方
* 导入第三方库：$pod install（更新太慢）
* 注意：请忽略升级CocoaPods的spec仓库
* pod install --verbose --no-repo-update
* pod update --verbose --no-repo-update

## 将代码提交到CocoaPods
 >http://www.cnblogs.com/wengzilin/p/4742530.html
