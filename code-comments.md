# 代码注释规范
当需要的时候，注释应该被用来解释 为什么 特定代码做了某些事情。所使用的任何注释必须保持最新，否则就删除掉。

通常应该避免一大块注释，代码就应该尽量作为自身的文档，只需要隔几行写几句说明。这并不适用于那些用来生成文档的注释。
## 文件注释
采用Xcode自动生成的注释格式，修改部分参数：

```objc
//
//  AppDelegate.m
//  oc code good
//
//  Created by roycms on 16-10-30.
//  Copyright (c) 2016 roycms.cn LLC. All rights reserved.
//

```
其中**项目名称**、**创建人**、**公司版权**需要填写正确。



## import注释
如果有一个以上的 import 语句，就对这些语句进行[分组][Import_1]。每个分组的注释是可选的。   
注：对于模块使用 [@import][Import_2] 语法。   

```objc  
	// Frameworks
	@import QuartzCore;

	// Models
	#import "NYTUser.h"

	// Views
	#import "NYTButton.h"
	#import "NYTUserView.h"
```   

## 方法注释
采用javadoc的格式，可以使用XCode插件VVDocumenter-Xcode快速添加，只需输入`///`即可

```objc
/**
 *  功能描述
 *
 *  @param tableView 参数说明
 *  @param section   参数说明
 *
 *  @return 返回值说明
 */
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
{
    return [self.familyNames objectAtIndex:section];
}
```
## 代码块注释
单行的用`//`+空格开头，多汗的采用`/*  */`注释
##TODO注释
TODO 很不错, 有时候, 注释确实是为了标记一些未完成的或完成的不尽如人意的地方, 这样一搜索, 就知道还有哪些活要干, 日志都省了。

格式：`//TODO:说明`

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    //TODO:增加初始化
    return YES;
}

```
