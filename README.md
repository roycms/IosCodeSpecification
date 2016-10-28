# iOS代码规范 个人总结

## 目录

1. oc命名规范
2. 代码规范
3. 框架使用规范


### 命名规范

命名规范推荐使用驼峰式。

**注意：** `请勿使用中文拼音来进行命名。`

|类型|格式|举例|
|---|---|---|
|类名|大驼峰|`NSString，NSArray，UIView`  |
|变量|小驼峰|`string，dataArray，userInfoView`|  
|函数|小驼峰|`- (void)addSubview:(UIView *)view;`  |
|宏定义|全大写|`VIEW_HEIGHT，VIEW_WIDTH`|

### 注释规范

开发过程中，注释需要好好的进行填写，特别是对函数的注释。一般情况下，源程序有效注释量必须在30%以上。 

注释分为两类

1. 函数注释
2. 执行代码注释

**注释参考**

```
/**
 *  打印函数
 */
- (void)log
{
    // 打印内容
    NSLog(@"Hello World");
}
```

### 其他注意事项

#### 函数解耦

* 当某一个函数内容特别庞大的时候，需要对内部功能进行拆简。
* 当多个函数有重复代码时，需要对其重复部分进行拆分。

#### 函数如果过长，参数过多需要进行折行处理

```
- (void)addUsername:(NSString *)username
           password:(NSString *)password;

```

#### 代码缩进（一个缩进为四空格）

请一定要在正确的地方使用缩进，这样可以提高代码的可阅读行。

#### 代码断行
 
代码在一行的列数请勿超过80（或100），超过需要进行断行。

提示设置

1. 打开Xcode
2. Command + ,
3. 选择`Text Editing`
4. 勾选`page guide at column`，在后面括号填入一行最大列数

#### 代码换行

`if`，`while`，`for`，需要在开始前与结束后加入换行，例如

```
- (void)func
{
    [self run];
    
    if (true) {
        // 上下需要换行
    }
    
    [self run];
}
```

#### 使用`#pragma mark -`

将函数归类，当实现类函数特别多的时候，可以通过一些特定的含义进行代码归类。

## 使用框架时，注意事项

### 继承框架提供的类

很多人在使用框架的时候，都会直接拿来其中的对象进行使用，其实这样对后续开发的扩展性带来了很大的影响。

因此，推荐大家最好继承要调用的对象，然后以它作为基类，然后一个项目将会有一套统一前缀的基类。

例如:

1. JXModel
2. JXView
3. JXController

#### 优点

1. 可以对原有的类进行扩展，重写。通过这些技巧来完成原有类无法完成的一些事情。
2. 添加前缀的好处是不会与一些其他的类进行重名，导致项目编译出错。

### 树形结构

* Class
    * AppDelegate 
	* View
		* BaseView
		* Folder
	* Controller
		* BaseController
		* Folder
	* Model
	  * BaseModel
	  * Folder
	* Utils
	  * Folder
	* Category
	  * Folder
	* Other
	  * Folder
 